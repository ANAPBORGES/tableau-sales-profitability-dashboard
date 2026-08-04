# Build Guide — publishing this dashboard on Tableau Public (free)

Step-by-step to turn the spec into a live, shareable dashboard. ~45–60 min.

## 0. Get Tableau (free)
- Download **[Tableau Public](https://public.tableau.com/en-us/s/download)** (free desktop app), or use Tableau Desktop if you have it.
- Create a free **[Tableau Public account](https://public.tableau.com/)** (needed to publish).

## 1. Connect the data
1. Open Tableau Public → **Connect ▸ Text file** → select [`data/superstore.csv`](../data/superstore.csv).
2. On the data-source tab, confirm data types: `Order Date`/`Ship Date` = **Date**; `Sales`/`Profit`/`Discount` = **Number (decimal)**; `Postal Code` = **String** (so it isn't summed).
3. Give `State` and `Country` the **Geographic Role** (right-click ▸ Geographic Role ▸ State / Country) so the map works.

## 2. Create the calculated fields
Open [`calculated_fields.md`](./calculated_fields.md) and create each field (**Analysis ▸ Create Calculated Field**). Start with `Profit Ratio` and `Profit or Loss`, then the table calculations.

## 3. Build the worksheets
Follow [`dashboard_spec.md`](./dashboard_spec.md), one sheet at a time. Order that works well:
KPI tiles → Map → Category table → Trend → Pareto → Discount scatter → Top customers.

Tips:
- **Map:** drag `State` to the view, then `Profit` to Colour; change mark type to *Map* (filled).
- **Diverging colour:** Colour legend ▸ Edit Colors ▸ pick a red–green diverging palette ▸ *Advanced* ▸ centre = 0.
- **Pareto:** put `Sub-Category` on Columns sorted by Sales desc, `Sales` on Rows, add `Cumulative % of Sales` as a second axis (dual axis), set it to a line.
- **BANs:** a worksheet with just the measure on Text; hide headers; enlarge the number.

## 4. Assemble the dashboard
1. New **Dashboard**, size *1200 × 900* (or *Automatic*).
2. Drag sheets in per the layout diagram; use *Tiled* objects and a title text object.
3. Add the `Region`, `Category`, `Year` filters; right-click each ▸ **Apply to Worksheets ▸ All Using This Data Source**.
4. Dashboard menu ▸ **Actions ▸ Add Filter** → set the **map** as a filter for the other sheets.

## 5. Publish
1. **File ▸ Save to Tableau Public As…** → sign in → name it *Executive Sales & Profitability — Superstore*.
2. It opens in the browser. Copy the public URL.
3. Add the link to this repo's `README.md` (replace the placeholder) and drop 1–2 screenshots into [`assets/`](../assets/).

## 6. Polish checklist
- [ ] Every sheet has a clear title and formatted numbers (currency / %).
- [ ] Tooltips are readable (no raw field names).
- [ ] One consistent diverging palette for profit across all sheets.
- [ ] Filters and the map-as-filter interaction work.
- [ ] Dashboard fits without scrollbars at the chosen size.
