# Rebuild Steps

## Relationships

| From (many) | To (one) | Column |
|---|---|---|
| `fact_store_monthly_profitability` | `dim_store` | `store_id` |
| `fact_store_monthly_profitability` | `dim_date` | `date_id` |
| `fact_daily_sales` | `dim_store` | `store_id` |
| `fact_daily_sales` | `dim_date` | `date_id` |
| `fact_daily_sales` | `dim_product` | `product_id` |

## Design

Coffee-themed: a cream page, roast-brown palette, applied through a JSON theme. Colours set by hand on a visual override the theme, so use **Reset to default** if a visual looks off.

| Colour | Hex | Use |
|---|---|---|
| Cream | `#F5EBDD` | Page background |
| Roast brown | `#6F4E37` | Main bars and lines |
| Caramel | `#C68B59` | Second series, highlights |
| Dark espresso | `#3B2418` | Strong emphasis |
| Sage | `#7A8B69` | Good results |
| Latte | `#D9B99B` | Soft accents |
| Terracotta | `#B5533C` | Warnings only (London, loss-making stores) |
| Panel fill | `#FBF5EA` | Panels and footer |
| Side strip | `#E6D5B8` | Navigation strip |
| Text dark / muted | `#34231E` / `#6B5547` | Title and body / subtitle and captions |

**Colour rule:** terracotta only for bad news and sage only for good news, so readers get the message before they read a label. Keep one colour per year on every page.

## Executive Summary layout

Canvas: **Format > Page > Canvas settings > Custom, 1280 x 840**. Turn on gridlines and snap to grid. Set positions in Format > Size and position.

| Element | X | Y | W | H |
|---|---|---|---|---|
| Side strip (fill `#E6D5B8`, no border) | 0 | 0 | 76 | 840 |
| Title text box (Georgia 28 bold) | 106 | 22 | 720 | 50 |
| Subtitle (Segoe UI 12, `#6B5547`) | 106 | 66 | 720 | 30 |
| Year slicer (Tile style, Select all + 2023 + 2024) | 980 | 26 | 270 | 48 |
| Card: Total revenue | 106 | 105 | 164 | 146 |
| Card: Net profit | 284 | 105 | 164 | 146 |
| Card: Profit margin | 106 | 265 | 164 | 146 |
| Card: Loss months | 284 | 265 | 164 | 146 |
| Coffee cup panel | 466 | 105 | 277 | 307 |
| Monthly profit line chart | 761 | 105 | 489 | 307 |
| Revenue mix donut | 106 | 430 | 352 | 308 |
| Margin by region bars | 476 | 430 | 404 | 308 |
| Weekday revenue columns | 898 | 430 | 352 | 308 |
| Footer panel | 106 | 756 | 1144 | 64 |

- Panel rectangles: fill `#FBF5EA`, 1 px `#CDBBA0` border, corner radius 14, no shadow. Place each chart on its panel and set the chart background to transparent
- Year slicer: turn **Single select** off and **Show "Select all"** on. The "Select all" tile means both years
- Text sizes: Power BI uses points (roughly pixels x 0.75)
- Photos: resize to about 400 x 400, save as JPG or PNG under about 200 KB. Use Fill scaling with a border and corner radius to make a circle
- Social logos: download from each brand page as PNG (about 24 x 24) and set **Format > Action > Web URL**

## Final checks

- Every chart matches [`verified_values.md`](verified_values.md)
- No text box is clipped (grey bars beside a text box mean overflow)
- Selection pane (View > Selection) shows no stray objects
