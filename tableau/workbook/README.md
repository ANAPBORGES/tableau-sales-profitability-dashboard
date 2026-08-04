# Tableau workbook — how to open it

`Superstore_Sales_Profitability.twb` is a Tableau workbook with the data source and every calculated field already defined. Open it and you can start dropping fields onto shelves — the setup work is done.

---

## Open it

1. **Tableau Desktop ▸ File ▸ Open** ▸ select `Superstore_Sales_Profitability.twb`
2. If Tableau asks where the data is, point it at [`../../data/superstore.csv`](../../data/superstore.csv). The workbook stores an absolute path, so it needs re-pointing on any machine but the one it was written on — **Data ▸ Superstore ▸ Edit Connection**.

Expected on connect: **9,994 rows**, 2015–2018, US$2,297,201 in Sales, US$286,397 in Profit.

---

## What is already set up

**21 typed columns**, with two choices worth knowing about:

- **`Postal Code` is a string, not a number.** Typed as a number it drops the leading zero on north-eastern ZIPs (`01star` → `1star`). A postal code is a label; it is never summed.
- **Geographic roles are assigned** — `Country`, `State`, `City` and `Postal Code` carry Tableau's geographic roles, so maps work immediately instead of showing "unknown" and asking you to assign them by hand.

**7 calculated fields**, each carrying a description (hover the field in the Data pane to read it):

| Field | What it is |
|---|---|
| `Profit Ratio` | `SUM([Profit]) / SUM([Sales])` — the backbone metric |
| `Profit or Loss` | Flag for diverging colour |
| `Avg Discount` | Row-level average discount |
| `YoY Sales Growth` | Table calc — set **Compute Using** to the date axis |
| `Running Total of Sales` | Table calc |
| `Cumulative % of Sales` | The Pareto curve |
| `3-Month Moving Average` | Trailing window: current month + two before |

### Two things that will bite if you skip them

**`Profit Ratio` is `SUM/SUM`, not `AVG` of a row-level ratio.** An average of per-row margins weights a US$5 line the same as a US$5,000 one and gives a number that is wrong in a way nobody catches.

**The four table calcs need Compute Using set on the sheet.** They are defined here, but a table calculation has no meaning until it knows which direction to run along. Dropped on a sheet without setting it, they return plausible nonsense — and `Cumulative % of Sales` in particular needs sub-categories **sorted descending** first, or the Pareto curve is not a Pareto curve.

---

## Then build the sheets

Follow [`../dashboard_spec.md`](../dashboard_spec.md) and [`../build_guide.md`](../build_guide.md).

---

## If it does not open

This workbook was authored as XML and **has not been opened in Tableau Desktop** — the format is version-sensitive. If Desktop refuses it, connect to [`../../data/superstore.csv`](../../data/superstore.csv) directly and paste the calculated fields from [`../calculated_fields.md`](../calculated_fields.md); it is the same setup, entered by hand.
