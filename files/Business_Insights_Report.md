# Business Insights & Recommendations
**Kaleido Lifestyle Retail Pvt Ltd (fictional) — synthetic data, Jan 2023 – Dec 2025**

> Every figure below is computed directly from `sales_data.csv` (22,000 order lines, 8,629 customers with delivered orders).
> **Definitions:** *Net Revenue* = Revenue of Delivered orders that were not returned. *Profit* = Revenue − Quantity × Cost (**gross profit**; store rent, salaries and marketing are not in the dataset). *Net Profit* = profit on the same kept orders. Cancelled orders are excluded everywhere; returned orders earn ₹0 net revenue and ₹0 net profit (goods assumed restocked).
> **Headline:** Net Revenue ₹59.82M · Net Profit ₹26.36M (44.1%) · 21,227 delivered orders · AOV ₹3,057 · Net revenue growth 11.0% (2024) and 9.7% (2025).

## Part A — The 15 questions

| # | Question | Finding (calculated from data) |
|---|---|---|
| 1 | Highest-revenue category? | **Accessories** — ₹17.93M (30.0% of net revenue). Fashion & Apparel is a close second at ₹17.08M (28.5%). |
| 2 | Highest-profit brand? | **Carryon Crafts** — ₹3.87M net profit at a 51.3% margin. Top-revenue brand Carryon Crafts (₹7.55M) ranks #1 on profit (₹3.87M, margin 51.3%). |
| 3 | Highest-sales city? | **Mumbai** — ₹8.79M (14.7%); Delhi is within 0.6% at ₹8.73M. |
| 4 | Best region? | **West** — ₹22.37M (37.4%). Note West contains 5 cities while East contains only Kolkata. Per-city average: West ₹4.47M, South ₹6.11M, North ₹4.75M, East ₹4.88M. |
| 5 | Age group spending most? | **25-34** (₹19.71M, 32.9%) — practically tied with 35-44 (₹19.66M). Together they are 65.8% of revenue; 55+ is only 2.3%. |
| 6 | Gender buying more Fashion? | **Female**: ₹8.82M (51.7%) and 2,582 orders vs Male ₹8.11M (47.5%), 2,377 orders. |
| 7 | Most profitable channel? | Margins are close — **Offline** is highest (44.3%) vs Online 44.0% and Mobile App 43.6%. The real gap is returns: Offline 3.7%, App 10.0%, Online 11.5%. Offline also earns ₹14.29M (54.2% of net profit). |
| 8 | Highest return rates? | Sub-category: **Jeans (18.5%)**, then Dresses (14.7%) and Jackets (14.5%). Product (≥40 orders): **Indigo Crest Heritage Jacket (28.6%)**, Threadwell Classic Denim Jeans (23.0%). Overall return rate 7.2%. |
| 9 | Discounts vs margin? | Margin falls steeply with discount depth: 0% off: 49.5% → 1-15% off: 44.8% → 16-25% off: 35.3% → 26-35% off: 23.4% → 36-50% off: 7.9%. 233 order lines were sold below cost — **100.0% of them in End-of-Season Sale**. |
| 10 | Highest-sales months? | **Nov (13.1%), Oct (12.9%), Dec (10.9%)**; weakest: Feb (6.3%), Jul (6.4%), Sep (6.4%). |
| 11 | Best festive season? | Oct–Nov **Festive season** yields ₹15.53M = 26.0% of 3-year revenue from 2 of 12 months. By promotion, End-of-Season Sale leads (₹4.28M) but at only 20.4% margin; **Diwali Festive Sale** is #2 (₹3.86M, margin 29.7%, flat at ≈₹1.29M/yr: 2023: ₹1.28M, 2024: ₹1.30M, 2025: ₹1.28M). Christmas Sale grew ₹0.66M → ₹0.72M. |
| 12 | Underperforming stores? | **8 of 24 stores** earn <75% of the ₹2.49M store average: Gangapur Road Outlet (₹0.33M), Dharampeth Outlet (₹0.54M), College Road Mall Store (₹0.55M), Gomti Nagar Outlet (₹0.75M), Sitabuldi Mall Store (₹1.07M), Vaishali Nagar Outlet (₹1.16M), Satellite High Street (₹1.57M), Hazratganj Mall Store (₹1.58M). Cities affected: Ahmedabad, Jaipur, Lucknow, Nagpur, Nashik; formats: High Street, Mall Store, Outlet. |
| 13 | Top customer segment? | **Trend Follower** — ₹18.90M (31.6%); Family Shopper 22.5%, Premium Shopper 22.2%. |
| 14 | Repeat purchase %? | **58.8%** of customers bought ≥2 times; they generate **84.1%** of net revenue. Top 10% of customers = 36.1% of revenue. |
| 15 | Categories needing improvement? | **Fashion & Apparel**: highest return rate (12.6% vs 7.2% overall) and 39.2% margin. **Footwear**: lowest margin (37.3%), 9.2% returns. **Sports & Fitness**: smallest category (5.1% share). |

## Part B — Extra insights
16. **Returns are costly:** 1,528 returned lines removed ₹5.06M revenue (7.8% of delivered revenue) and ₹2.15M profit (7.5% of gross profit).
17. **Digital is growing:** Online + Mobile App share of orders rose 44.2% → 49.0% → 54.6% (2023→2025).
18. **Cancellations are a digital problem:** Online 6.1%, Mobile App 5.2%, Offline 1.4%.
19. **Beauty & Personal Care has the best unit economics:** 61.5% margin and the lowest return rate (2.9%), but only 7.5% of revenue.
20. **Growth is steady, seasonality strong:** net revenue ₹17.97M → ₹19.95M → ₹21.89M.

## Part C — Recommendations (each tied to a finding above)
1. **Cap End-of-Season discounts at ≈30%.** The 36–50% band earns a 7.9% margin vs 35.3% at 16–25%, and every one of the 233 loss-making lines came from this sale (Insight 9).
2. **Fix Jeans, Dresses and Jacket returns** (e.g. size-guides, fit-model photos, fabric detail) — they have the highest return rates (18.5%, 14.7%, 14.5%). Each point of Fashion return rate recovered is worth roughly ₹0.20M revenue (Insight 8, 15).
3. **Review or delist the worst-returning SKUs** such as Indigo Crest Heritage Jacket (28.6%) (Insight 8).
4. **Tighten online/app return policy and Net-Banking payment checks:** online/app return rates are 11.5%/10.0% vs 3.7% in store; Net Banking orders return at 11.5% (Insight 7, 16).
5. **Reduce online/app cancellations** (≈6.1%/5.2%) via stock-sync and delivery-promise accuracy (Insight 18).
6. **Plan Oct–Nov inventory and staffing early:** those two months deliver 26.0% of revenue; protect stock of Lighting, Home Decor, Furnishing (highest Oct–Nov share of their own annual sales: 37.2%, 35.1%, 33.9%) (Insight 10, 11).
7. **Replace blanket Diwali discounts with targeted offers:** Diwali-promo revenue is flat (≈₹1.29M/yr) while total revenue grows — test depth vs reach on Loyalty/Premium customers (Insight 11).
8. **Turn the 41.2% one-time buyers into repeat customers** (post-purchase journeys, loyalty points): repeat customers drive 84.1% of revenue (Insight 14).
9. **Review the 8 underperforming stores** — consider format change (Outlet → shop-in-shop), local marketing, or ship-from-store focus for online orders (Insight 12).
10. **Expand Beauty & Personal Care assortment and cross-sell it to Fashion/Accessories buyers** — highest margin (61.5%), lowest returns (Insight 19).
11. **Grow Sports & Fitness through bundles with Footwear/Sportswear** — smallest category (5.1%) with a 41.6% margin (Insight 15).
12. **Target 18-24 and 45+ segments** with campaigns: 25–44 customers already supply 65.8% of revenue while 55+ supplies only 2.3% (Insight 5).

## Part D — Limitations (read before presenting)
* The data is **synthetic**; patterns (seasonality, discount depth, return propensities, store differences) were *designed into the generator*. Treat findings as a demonstration of analytical method, not as real-market evidence.
* Basket size was **not** modelled as discount-elastic, so this dataset cannot answer whether discounts lift volume.
* "Profit" excludes operating costs, so margins (≈44%) are gross margins.
* East region = Kolkata only; online orders are attributed to the customer's city store (fulfilment store).
