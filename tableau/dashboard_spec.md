# Dashboard Specification — Executive Sales & Profitability

A single interactive dashboard (plus supporting sheets) telling one story:
**where we make money, where we lose it, and what discounting does to profit.**

Data: [`data/superstore.csv`](../data/superstore.csv) · Sample Superstore, 2015–2018,
US$2.30M sales · US$286K profit · 12.5% margin.

---

## Worksheets

### 1. KPI tiles (4 BANs)
Big Ascii Numbers, one per tile: `SUM(Sales)`, `SUM(Profit)`, `Profit Ratio`, `COUNTD(Order ID)`.
Small text; large value. Colour `Profit` / `Profit Ratio` green.

### 2. Profit by State — filled map ⭐
- Marks: **Map** (filled), Geographic role on `State`.
- Colour: `SUM(Profit)`, **diverging red–green centred on 0**.
- Tooltip: State, Sales, Profit, Profit Ratio.
- **Story:** 10 of 49 states lose money — Texas (−$26K) and Pennsylvania (−$16K) are red despite high sales.

### 3. Category / Sub-Category profitability — highlight table
- Rows: `Category` › `Sub-Category`. Text: `SUM(Sales)`. Colour: `Profit Ratio` (diverging).
- **Story:** Furniture sells a lot but barely profits (2.5% ratio); *Tables* and *Bookcases* are red.

### 4. Sales & profit trend — dual axis
- Columns: `MONTH(Order Date)`. Bars: `SUM(Sales)`; line: `3-Month Moving Average` (or `Profit Ratio`).
- Optional: `YoY Sales Growth` label. **Story:** steady growth, ~+51% over the period.

### 5. Sub-category Pareto — combo
- Bars: `SUM(Sales)` by `Sub-Category` (sorted desc). Line: `Cumulative % of Sales`.
- Reference line at 80%. **Story:** top 6 of 17 sub-categories ≈ 65% of sales.

### 6. Discount vs Profit — scatter + what-if
- Marks: circles per `Sub-Category` (or order bucket). X: `Avg Discount`; Y: `Profit Ratio`; size: `Sales`.
- Add the `What-if Discount` parameter + `Simulated Profit` line on a companion sheet.
- **Story:** profit turns negative above ~20% discount; >30% discount = 83% loss rate.

### 7. Top customers — bar (LOD)
- Rows: `Customer Name` (top N by `Customer Sales (lifetime)`), colour by `Customer Value Tier`.

---

## Dashboard layout (1200 × 900, tiled)

```
┌──────────────────────────────────────────────────────────────┐
│  Title: Executive Sales & Profitability — Superstore 2015–18   │
├───────────┬───────────┬───────────┬───────────────────────────┤
│  Sales    │  Profit   │ Profit %  │  Orders      (4 KPI tiles) │
├───────────────────────────────┬──────────────────────────────┤
│                                │  Category / Sub-Category      │
│   Profit by State (map)        │  profitability (highlight)    │
│                                │                               │
├───────────────────────────────┼──────────────────────────────┤
│   Sales & Profit trend         │  Sub-category Pareto          │
├───────────────────────────────┴──────────────────────────────┤
│   Discount vs Profit scatter  +  [What-if Discount] parameter  │
└──────────────────────────────────────────────────────────────┘
```

**Global filters (apply to all):** `Region`, `Category`, `YEAR(Order Date)`.

**Interactions:**
- Map → **Use as Filter** (click a state filters every sheet).
- `Region` filter set to *Apply to all using this data source*.
- `What-if Discount` parameter drives the simulation sheet only.
- Highlight action on `Category` between the table and the trend.

---

## Colour & style
- One diverging palette (red→grey→green) for all profit encodings, centred on 0.
- One sequential palette for sales-only encodings.
- Neutral background, consistent number formatting (see [`calculated_fields.md`](./calculated_fields.md)).
- Fonts: Tableau Book; titles bold. Keep it executive: few colours, clear hierarchy.
