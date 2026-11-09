# Power Query (M) — Cleaning pipeline
Mirrors `scripts/gen.py → clean()` and the Cleaning_Log. Load `raw_sales_data.csv` as `Raw`; load `customers.csv`, `products.csv`, `stores.csv` as `Customers`, `Products`, `Stores` first.
```m
let
  Source   = Csv.Document(File.Contents("C:\Retail_BI_Project\data\raw_sales_data.csv"),[Delimiter=",",Encoding=65001,QuoteStyle=QuoteStyle.Csv]),
  Promoted = Table.PromoteHeaders(Source,[PromoteAllScalars=true]),
  // 1 Trim & clean all text
  Trimmed  = Table.TransformColumns(Promoted,{},each if _ is text then Text.Trim(Text.Clean(_)) else _),
  // 2 Standardise categories / channel / city
  Std1 = Table.TransformColumns(Trimmed,{{"Category",each Text.Proper(_)},{"Sales_Channel",each Text.Proper(_)}}),
  Std2 = Table.ReplaceValue(Std1,"Bangalore","Bengaluru",Replacer.ReplaceValue,{"City"}),
  Std3 = Table.ReplaceValue(Std2,"Fashion & apparel","Fashion & Apparel",Replacer.ReplaceValue,{"Category"}),
  Std4 = Table.ReplaceValue(Std3,"Beauty & personal care","Beauty & Personal Care",Replacer.ReplaceValue,{"Category"}),
  Std5 = Table.ReplaceValue(Std4,"Home & lifestyle","Home & Lifestyle",Replacer.ReplaceValue,{"Category"}),
  Std6 = Table.ReplaceValue(Std5,"Sports & fitness","Sports & Fitness",Replacer.ReplaceValue,{"Category"}),
  // 3 Remove duplicates on the key
  Dedup = Table.Distinct(Std6,{"Order_ID"}),
  // 4 Dates: accept ISO or dd-MM-yyyy
  ToDate = (v) => if v=null or v="" then null else try Date.FromText(v,[Format="yyyy-MM-dd",Culture="en-IN"]) otherwise Date.FromText(v,[Format="dd-MM-yyyy",Culture="en-IN"]),
  Dates = Table.TransformColumns(Dedup,{{"Order_Date",ToDate,type date},{"Ship_Date",ToDate,type date}}),
  // 5 Numbers stored as text (₹, commas)
  Clean = (v) => if v=null or v="" then null else Number.From(Text.Replace(Text.Replace(Text.Replace(v,"₹",""),",","")," ","")),
  Nums = Table.TransformColumns(Dates,{{"Unit_Price",Clean,type number},{"Revenue",Clean,type number},{"Quantity",Clean,Int64.Type},{"Discount",Clean,type number},{"Cost_Price",Clean,type number}}),
  // 6 Discount on 0-100 scale
  Disc = Table.TransformColumns(Nums,{{"Discount",each if _>1 then _/100 else _,type number}}),
  // 7 Restore master attributes (fixes missing / outlier Age, Gender, Name, Brand, City, Unit_Price)
  JC = Table.NestedJoin(Disc,"Customer_ID",Customers,"Customer_ID","C",JoinKind.LeftOuter),
  XC = Table.ExpandTableColumn(Table.RemoveColumns(JC,{"Customer_Name","Gender","Age","Age_Group","Customer_Segment","Customer_Loyalty_Status"}),"C",{"Customer_Name","Gender","Age","Age_Group","Customer_Segment","Customer_Loyalty_Status"}),
  JP = Table.NestedJoin(XC,"Product_ID",Products,"Product_ID","P",JoinKind.LeftOuter),
  XP = Table.ExpandTableColumn(Table.RemoveColumns(JP,{"Product_Name","Brand","Category","Sub_Category","Unit_Price","Cost_Price"}),"P",{"Product_Name","Brand","Category","Sub_Category","Unit_Price","Cost_Price"}),
  JS = Table.NestedJoin(XP,"Store_ID",Stores,"Store_ID","S",JoinKind.LeftOuter),
  XS = Table.ExpandTableColumn(Table.RemoveColumns(JS,{"Store_Name","Store_Type","City","State","Region"}),"S",{"Store_Name","Store_Type","City","State","Region"}),
  // 8 Invalid quantity: re-derive from revenue; recompute money columns
  Qty  = Table.TransformColumns(XS,{},each _),
  QFix = Table.AddColumn(Qty,"Q2",each if [Quantity]=null or [Quantity]<=0 then Number.Round([Revenue]/([Unit_Price]*(1-[Discount]))) else [Quantity],Int64.Type),
  Q3   = Table.RenameColumns(Table.RemoveColumns(QFix,{"Quantity"}),{{"Q2","Quantity"}}),
  Rev  = Table.TransformColumns(Table.AddColumn(Q3,"Rv",each Number.Round([Quantity]*[Unit_Price]*(1-[Discount]),2),type number),{}),
  Rev2 = Table.RenameColumns(Table.RemoveColumns(Rev,{"Revenue"}),{{"Rv","Revenue"}}),
  Prof = Table.AddColumn(Rev2,"Profit2",each Number.Round([Revenue]-[Quantity]*[Cost_Price],2),type number),
  Prof2= Table.RenameColumns(Table.RemoveColumns(Prof,{"Profit"}),{{"Profit2","Profit"}}),
  Marg = Table.AddColumn(Prof2,"Profit_Margin",each Number.Round([Profit]/[Revenue],4),type number),
  // 9 Ship date earlier than order date: swap
  Swap = Table.TransformColumns(Table.AddColumn(Marg,"flag",each [Ship_Date]<>null and [Ship_Date]<[Order_Date]),{}),
  OD = Table.AddColumn(Swap,"OD",each if [flag] then [Ship_Date] else [Order_Date],type date),
  SD = Table.AddColumn(OD,"SD",each if [flag] then [Order_Date] else [Ship_Date],type date),
  Fin = Table.RenameColumns(Table.RemoveColumns(SD,{"Order_Date","Ship_Date","flag"}),{{"OD","Order_Date"},{"SD","Ship_Date"}}),
  // 10 Derived columns
  GS = Table.AddColumn(Fin,"Gross_Sales",each Number.Round([Quantity]*[Unit_Price],2),type number),
  DA = Table.AddColumn(GS,"Discount_Amount",each Number.Round([Gross_Sales]-[Revenue],2),type number),
  NR = Table.AddColumn(DA,"Net_Revenue",each if [Order_Status]="Delivered" and [Return_Status]="Not Returned" then [Revenue] else 0,type number),
  NP = Table.AddColumn(NR,"Net_Profit",each if [Order_Status]="Delivered" and [Return_Status]="Not Returned" then [Profit] else 0,type number)
in NP
```
After loading, reorder columns to match `data_dictionary.xlsx`. Row counts should be 22,000 (from 22,180). Outliers: flag with Table.Profile / Column Quality; high-value orders are retained (see Cleaning_Log).
