# DAX Measures, Date Table & Validation
Model: `Sales` (fact, from sales_data.csv) → `Customers`, `Products`, `Stores` (dimensions, many-to-one on Customer_ID / Product_ID / Store_ID) and `Date` (Sales[Order_Date] → Date[Date]). Single-direction relationships, Date marked as date table.
Business rules: cancelled orders excluded; *Total Revenue* = delivered (before returns); *Net Revenue* = delivered and not returned; profit = gross profit. Put all measures in a `_Measures` table.

## Date table
```DAX
Date =
VAR _min = DATE(YEAR(MIN(Sales[Order_Date])),1,1)
VAR _max = DATE(YEAR(MAX(Sales[Order_Date])),12,31)
RETURN ADDCOLUMNS(
  CALENDAR(_min,_max),
  "Year", YEAR([Date]), "Month No", MONTH([Date]), "Month", FORMAT([Date],"mmm"),
  "Year-Month", FORMAT([Date],"yyyy-mm"), "Quarter", "Q" & QUARTER([Date]),
  "Year-Quarter", YEAR([Date]) & " Q" & QUARTER([Date]), "Weekday", FORMAT([Date],"ddd"),
  "Season", SWITCH(TRUE(), MONTH([Date]) IN {12,1,2},"Winter", MONTH([Date]) IN {3,4,5},"Summer", MONTH([Date]) IN {6,7,8,9},"Monsoon","Festive"))
```
Set `Month` to sort by `Month No`; mark table as date table on `[Date]`.

## Measures
| # | Measure | DAX | Explanation / validation |
|---|---|---|---|
| 1 | Total Revenue | `CALCULATE(SUM(Sales[Revenue]), Sales[Order_Status]="Delivered")` | Gross of returns. Check: ₹6.49 Cr (64,880,533.40) |
| 2 | Net Revenue | `CALCULATE(SUM(Sales[Revenue]), Sales[Order_Status]="Delivered", Sales[Return_Status]="Not Returned")` | = SUM(Sales[Net_Revenue]) → 59,816,594.95 |
| 3 | Total Profit | `CALCULATE(SUM(Sales[Profit]), Sales[Order_Status]="Delivered", Sales[Return_Status]="Not Returned")` | = SUM(Sales[Net_Profit]) → 26,360,010.34 |
| 4 | Total Orders | `CALCULATE(COUNTROWS(Sales), Sales[Order_Status]="Delivered")` | 21,227 |
| 5 | Total Customers | `CALCULATE(DISTINCTCOUNT(Sales[Customer_ID]), Sales[Order_Status]="Delivered")` | 8,629 |
| 6 | Total Quantity | `CALCULATE(SUM(Sales[Quantity]), Sales[Order_Status]="Delivered")` | 34,614 |
| 7 | Average Order Value | `DIVIDE([Total Revenue],[Total Orders])` | 3,056.51 |
| 8 | Average Selling Price | `DIVIDE([Total Revenue],[Total Quantity])` | revenue per unit after discount |
| 9 | Profit Margin % | `DIVIDE([Total Profit],[Net Revenue])` | 44.07% |
| 10 | Returning Customers | `COUNTROWS(FILTER(VALUES(Sales[Customer_ID]), CALCULATE(COUNTROWS(Sales), Sales[Order_Status]="Delivered")>=2))` | customers with ≥2 delivered orders (context-aware) |
| 11 | Repeat Customer Rate | `DIVIDE([Returning Customers],[Total Customers])` | 58.8% overall |
| 12 | Previous Year Revenue | `CALCULATE([Net Revenue], SAMEPERIODLASTYEAR('Date'[Date]))` | Net revenue basis |
| 13 | YoY Growth | `DIVIDE([Net Revenue]-[Previous Year Revenue],[Previous Year Revenue])` | 2024: +11.0%, 2025: +9.7% |
| 14 | MoM Growth | `VAR _p = CALCULATE([Net Revenue], DATEADD('Date'[Date],-1,MONTH)) RETURN DIVIDE([Net Revenue]-_p,_p)` | compare with Excel Monthly_Analysis col I |
| 15 | YTD Sales | `TOTALYTD([Net Revenue],'Date'[Date])` | Dec-2025 YTD = 2025 net revenue |
| 16 | MTD Sales | `TOTALMTD([Net Revenue],'Date'[Date])` | |
| 17 | Return Rate | `DIVIDE(CALCULATE(COUNTROWS(Sales), Sales[Order_Status]="Delivered", Sales[Return_Status]="Returned"),[Total Orders])` | 7.2% |
| 18 | Cancellation Rate | `DIVIDE(CALCULATE(COUNTROWS(Sales), Sales[Order_Status]="Cancelled"), COUNTROWS(Sales))` | 3.5% |
| 19 | Discount % | `DIVIDE(CALCULATE(SUM(Sales[Discount_Amount]),Sales[Order_Status]="Delivered"), CALCULATE(SUM(Sales[Gross_Sales]),Sales[Order_Status]="Delivered"))` | 9.8% |
| 20 | Revenue Contribution % | `DIVIDE([Net Revenue], CALCULATE([Net Revenue], ALLSELECTED()))` | share of visible total; sums to 100% in any visual |

Extras: `Gross Profit = CALCULATE(SUM(Sales[Profit]),Sales[Order_Status]="Delivered")`; `Gross Revenue = [Total Revenue]`; `Return Revenue Lost = CALCULATE(SUM(Sales[Revenue]),Sales[Return_Status]="Returned")`; `Return Profit Lost = CALCULATE(SUM(Sales[Profit]),Sales[Return_Status]="Returned")`; `Margin Before Returns = DIVIDE([Gross Profit],[Total Revenue])`.
Discount band (calculated column): `Discount Band = SWITCH(TRUE(), Sales[Discount]=0,"0%", Sales[Discount]<=0.15,"1-15%", Sales[Discount]<=0.25,"16-25%", Sales[Discount]<=0.35,"26-35%","36-50%")`.

## How to validate
1. Card visuals with no filters must equal the "Check" values above (they are also in the Excel Sales_Analysis sheet and CSV pandas totals).
2. Compare a matrix (Category × Year) with Excel `Pivot_Tables` sheet cell by cell.
3. Selecting Year=2024 must give Net Revenue 19,953,409.70; YoY must show +11.0%.
4. Revenue Contribution % in any single-dimension table must total 100%.
5. Row-level test: filter Order_ID and verify Revenue = Qty × Price × (1 − Discount).
