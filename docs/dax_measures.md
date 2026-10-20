# DAX Measures and Calculated Columns

`fact_store_monthly_profitability` is abbreviated **FMP** below. In Power BI, write the full table name.

## Which pattern to use

| Pattern | When | Example |
|---|---|---|
| `SUM(column)` | You need an amount | Net Profit |
| `DIVIDE(SUM(a), SUM(b))` | The number is a percentage or a "per" figure | Profit Margin % |
| `CALCULATE(..., condition)` | You only want some of the rows | Profit 2023 |
| `[A] - [B]` | You are comparing two numbers | Profit Change |

## Profitability (FMP)

```DAX
Net Profit = SUM(fact_store_monthly_profitability[net_profit_gbp])

Total Revenue = SUM(fact_store_monthly_profitability[total_monthly_revenue_gbp])

Profit Margin % =
DIVIDE(
    SUM(fact_store_monthly_profitability[net_profit_gbp]),
    SUM(fact_store_monthly_profitability[total_monthly_revenue_gbp])
)

Loss Months % =
DIVIDE(
    CALCULATE(
        COUNTROWS(fact_store_monthly_profitability),
        fact_store_monthly_profitability[profitability_status] IN {"Loss-Making", "Structural-Loss"}
    ) + 0,
    COUNTROWS(fact_store_monthly_profitability)
)

Rent % of Revenue =
DIVIDE(
    SUM(fact_store_monthly_profitability[monthly_rent_gbp]),
    SUM(fact_store_monthly_profitability[total_monthly_revenue_gbp])
)

Profit 2023 =
CALCULATE(SUM(fact_store_monthly_profitability[net_profit_gbp]), dim_date[year] = 2023)

Profit 2024 =
CALCULATE(SUM(fact_store_monthly_profitability[net_profit_gbp]), dim_date[year] = 2024)

Profit Change = [Profit 2024] - [Profit 2023]
```

`+ 0` in Loss Months % shows 0.0% instead of blank.

## Loyalty (FMP)

```DAX
Member Revenue % =
DIVIDE(
    SUM(fact_store_monthly_profitability[loyalty_member_spend_gbp]),
    SUM(fact_store_monthly_profitability[total_monthly_revenue_gbp])
)

Redemption % of Member Revenue =
DIVIDE(
    SUM(fact_store_monthly_profitability[loyalty_redemption_cost_gbp]),
    SUM(fact_store_monthly_profitability[loyalty_member_spend_gbp])
)

Loyalty Net Value =
SUM(fact_store_monthly_profitability[loyalty_member_spend_gbp])
    - SUM(fact_store_monthly_profitability[loyalty_redemption_cost_gbp])

-- Test measure: proves member revenue in pounds equals loyalty_member_spend_gbp
Member Revenue Check =
SUMX(
    fact_store_monthly_profitability,
    fact_store_monthly_profitability[loyalty_member_revenue_pct]
        * fact_store_monthly_profitability[total_monthly_revenue_gbp]
)
```

## Daily sales (sample)

```DAX
Avg Transaction Value =
DIVIDE(SUM(fact_daily_sales[total_revenue_gbp]), SUM(fact_daily_sales[total_transactions]))

Loyalty Transaction Share =
DIVIDE(SUM(fact_daily_sales[loyalty_transactions]), SUM(fact_daily_sales[total_transactions]))

Espresso Share =
DIVIDE(SUM(fact_daily_sales[espresso_revenue_gbp]), SUM(fact_daily_sales[total_revenue_gbp]))

-- Same pattern for the other three streams:
-- Filter Share  -> filter_revenue_gbp
-- Food Share    -> food_revenue_gbp
-- Beans Share   -> retail_beans_revenue_gbp

Avg Daily Revenue = AVERAGE(fact_daily_sales[total_revenue_gbp])
```

## Calculated columns

```DAX
-- dim_store
Adoption Band =
SWITCH(
    TRUE(),
    dim_store[loyalty_adoption_rate] <= 0.2, "1 Low (up to 20%)",
    dim_store[loyalty_adoption_rate] <= 0.4, "2 Mid (20-40%)",
    "3 High (over 40%)"
)

-- dim_date
Day Number = WEEKDAY(dim_date[full_date], 2)      -- Monday = 1 ... Sunday = 7
Day Type   = IF(dim_date[is_weekend], "Weekend", "Weekday")
```

The leading numbers in `Adoption Band` keep the sort order. Use `Day Number` with **Sort by column** on `day_of_week`.

## Margin Cup (SVG)

`Cup SVG` reads `Profit Margin %`, works out how full the cup should be (full at a 30% margin) and returns a small SVG image as text. Paste your final formula below.

```DAX
Cup SVG = -- paste measure here
```

To show it: add a Table visual, put the measure in it, and set **Data category** to **Image URL**. Pair it with a region slicer. Power BI cannot animate it, so it updates when a viewer filters or clicks.

## Model settings that matter

- Mark `dim_date` as the date table (`full_date`)
- Format money columns as £ (not $)
- Sort `month_name` by `month_number`
- Set `year` to **Don't summarize**
- Set the count of `store_id` to **Count (Distinct)** when counting from the monthly table
