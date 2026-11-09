# Lifestyle Retail Sales & Business Intelligence Dashboard
**Fictional company: Kaleido Lifestyle Retail Pvt Ltd · ALL DATA IS SYNTHETIC**

| Deliverable | File |
|---|---|
| Live interactive dashboard (8 pages, 13 slicers) | dashboard/index.html (published link) |
| Excel workbook (14 sheets, ~200k live formulas, charts, dropdown-driven dashboard) | excel/Retail_Sales_Analytics_Workbook.xlsx |
| Data (22,000 orders, 8,736 customers, 132 products, 24 stores) | data/sales_data.csv/.xlsx, customers.csv, products.csv, stores.csv |
| Raw (dirty) data + cleaning log | data/raw_sales_data.csv, cleaning_log.csv |
| Data dictionary & relationships | data/data_dictionary.xlsx |
| 20 insights + 12 recommendations (all computed) | docs/Business_Insights_Report.md |
| DAX (20 measures + date table), Power Query M, Power BI build guide | docs/ |
| Reproducible code | scripts/ (run gen.py → insights.py → build_xlsx.py → dd.py → mkdash.py) |

## Honest limitations
* No `.pbix` is included (cannot be authored outside Power BI Desktop); the DAX/M/guide let you build it, and the HTML dashboard mirrors it.
* Excel native PivotTables/Slicers are not pre-built (openpyxl limitation); formula-driven cross-tabs plus Year/Region dropdown selectors are provided, with steps to add native ones.
* XLOOKUP is shown as text; INDEX/MATCH and VLOOKUP are used so the workbook calculates everywhere.
* Patterns in the data were designed into the generator; present findings as method demonstration, not market evidence.
