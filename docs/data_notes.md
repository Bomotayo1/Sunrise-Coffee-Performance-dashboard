# Data Notes

## Where the real files differ from the data dictionary

| The documents say | The data actually shows |
|---|---|
| Three formats: High-Street, Retail-Park, Transport-Hub | Only two: 58 High-Street and 2 Transport-Hub (both Luton train stations). No Retail-Park |
| `net_profit_margin_pct` ranges from -1.0 to 0.6 | It reaches -3.39, so there are extreme outliers |
| A few nulls in the monthly table | No nulls in the monthly table. The only blanks are 36 `launch_date` values in `dim_product` |
| `dim_date` has a `month_id` column | It does not. The monthly fact links to `dim_date` through `date_id` |
| Gross margin range 0.36 to 0.80 | Actual range is about 0.66 to 0.90 |
| Three trading-calendar values | Four: Standard-Trading-Day, Bank-Holiday, Christmas-Period, School-Holiday |
| Rent bands include Premium | No Premium rows. Stores fall in Low, Medium and High |
| Product status includes Discontinued and In-Test | Only Active (36) and Seasonal (4) |

## Traps to remember

- **The daily table is a sample** (10,000 of a possible 43,800 rows). Use it for ratios and patterns, never for monthly or annual totals
- **`pct` columns are fractions.** 0.15 means 15%. Format as Percentage
- **Status is per store-month**, so summarise it (for example, the share of months in loss)
- **`store_status` is a different field** (Active, Temporarily-Closed, Refurbishing). Check it before recommending closures
- **The daily table records only each day's top product**, so true category margins cannot be weighted by sales. Use `dim_product` for margin comparisons
- **Loyalty net value is crude.** It treats all member revenue as extra, but members may have bought anyway
- **Two bank holiday flags exist.** `is_uk_bank_holiday` marks 17 dates; `uk_trading_calendar = Bank-Holiday` covers 13. State which one you use
- **Count vs Count (Distinct) on `store_id`.** From `dim_store` they match. From the monthly table they differ, so use Count (Distinct)
- **Watch for hidden filters.** A selection limited to High-Street once dropped Luton Express and Luton House (the only Transport-Hubs) from a total
