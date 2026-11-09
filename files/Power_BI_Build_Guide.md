# Power BI Build Guide (8 pages)
A `.pbix` cannot be generated outside Power BI Desktop, so this project ships everything needed to build it in ~1 hour, plus a fully working web dashboard (dashboard/index.html) that implements the same 8 pages and 13 slicers.

1. **Get data:** import sales_data.csv, customers.csv, products.csv, stores.csv (or run Power_Query_M_Script.md on raw_sales_data.csv). Set types; Discount/Profit_Margin as Percentage.
2. **Model:** relationships Sales→Customers/Products/Stores (many-to-one, single), Sales[Order_Date]→Date[Date]; create Date table and measures from DAX_Measures.md.
3. **Theme:** navy #1F2A44, teal #2A7F8E, gold #C8A24A, grey #F3F4F6; font Segoe UI; background #F5F6F8; KPI cards with 4px accent bar.
4. **Slicers (sync across pages via View → Sync slicers):** Year, Month, Region, State, City, Category, Brand, Product, Gender, Age_Group, Sales_Channel, Store_Name, Payment_Method.
5. **Pages** (visuals map 1:1 to dashboard/index.html):
| Page | Visuals |
|---|---|
| 1 Executive Overview | Cards: Total Revenue, Total Profit, Total Orders, Total Customers, AOV, Profit Margin %; line Net Revenue & Profit by Year-Month; bar Category; column Region |
| 2 Category & Brand | Bar Category/Sub-Category; Top N filter 10 / Bottom N 10 Brand; Margin by Category; Top 10 products |
| 3 Customer Analytics | Donut Segment & Gender; column Age_Group; Loyalty; Repeat Customer Rate card; Top 10 customers table |
| 4 Store & Regional | Filled map or bar by City/State; Region margin; Top/Bottom N stores |
| 5 Online vs Offline | Donut Revenue by channel; Profit, Orders, AOV by channel |
| 6 Product Performance | Top/Bottom N products by Quantity; Return Rate by product (Top N, filter orders ≥ 40) |
| 7 Profitability & Returns | Cards Gross/Net Revenue, Profit, Margin, Return/Cancellation Rate; Margin by Discount Band; Return Revenue/Profit Lost by Category |
| 8 Seasonal & Festive | Month, Quarter, Year; Season; Promotion_Type (Diwali, Christmas, EOSS); YoY Growth matrix |
6. **Publish** to Power BI Service → File → Publish to web (for a public portfolio link) — note this requires your own account and exposes the report publicly (fine: data is synthetic).
