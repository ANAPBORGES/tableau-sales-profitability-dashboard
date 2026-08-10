# Finishing guide — what is left

The workbook ships built: 20 worksheets, 5 dashboards, explicit sorts on every bar chart,
diverging colour centred on zero, reference lines at zero and at 80%, the header frame and
the footers. **Three things remain**, and each is faster to do by hand than to specify.

## 1. Save as `.twbx` before anything else (1 min)

The workbook carries a data extract, and Tableau put it in the Windows temp folder — a
folder Windows eventually clears. A `.twb` also stores the five layout images as *paths*, so
they break for anyone else who opens it. **File ▸ Save As ▸ .twbx** packages both.

## 2. Re-fit the header band (1 min)

The header images were first rendered at 2x (2400 x 112) for sharpness. The cover scales to
its zone, but the band was drawn at natural size and **centre-cropped** — the logo fell
outside the crop and the nav labels came out at double size. They are now 1:1 (1200 x 56),
which is exactly the zone, so natural size is the right size.

Reopen the `.twb` so it picks up the new files, check the band, then save the `.twbx` again.

## 3. Viz in tooltip on the map (5 min)

The single feature that turns the map from a picture into an instrument.

1. Open `Profit by State` ▸ Marks card ▸ **Tooltip**
2. **Insert ▸ Sheets ▸ `Profit by Sub-Category`**
3. Edit the inserted tag to `maxwidth="320" maxheight="260"`
4. Delete the rest of the default tooltip text except State, Sales, Profit

Hovering a state then answers "why is Texas red?" under the cursor, without a click. Keep it
under ~320×260 — a tooltip that fills the screen is a modal, and it covers the thing being
pointed at.

## 4. Navigation buttons (3 min)

The Home page navigates through the workbook tabs. For real buttons, drag a **Navigation**
object onto each dashboard, set its target and label it.

---

## Before publishing

- [ ] Saved as `.twbx`
- [ ] It reads correctly both filtered and unfiltered — including the title and the closing line
- [ ] Clicking a mark filters the page on every dashboard
- [ ] Link pasted into the README

---

## What was done, and why — kept for the record

## 1. Sort every bar chart (5 min, biggest payoff)

An unsorted bar chart is the single clearest sign that nobody looked at the output. The reader's first job — "who is biggest?" — should cost no effort.

For each of: `Profit by Sub-Category`, `Segment Performance`, `Ship Mode Performance`, `Discount by Category`, `Customers by Segment`, `Sales per Customer by Region`, `Avg Days to Ship`:

> Right-click the dimension pill ▸ **Sort** ▸ By **Field** ▸ **Descending** ▸ pick the measure on the shelf.

Use the explicit field sort, not the toolbar's quick sort — the quick sort is stored more loosely and can be lost when the sheet moves onto a dashboard.

**One exception:** `Profit by Sub-Category` should sort **ascending**, so the loss-makers land at the top where the eye starts. The chart exists to name them.

---

## 2. Give each sheet a title in business language (10 min)

Sheet names are internal labels; dashboard titles are what a director reads. Double-click each title on the dashboard and rewrite it as the finding, not the field list:

| Sheet | Title to use |
|---|---|
| Profit by State | Where we make and lose money |
| Category Profitability | Sales volume vs margin, by line |
| Discount vs Margin | What discounting costs us |
| Margin Trend | Margin over time — is growth profitable? |
| Pareto | How concentrated is revenue? |
| Region x Category | Which market–line combinations work |
| Profit by Sub-Category | The loss-makers, named |
| Segment Performance | Which segment pays |
| Customers by Segment | Customer base and average ticket |
| Sales per Customer by Region | Value per customer, by market |
| Ship Mode Performance | Delivery mode: volume vs margin |
| Avg Days to Ship | How long each mode actually takes |
| Discount by Category | Where the discounting is concentrated |

A title phrased as a question or a claim tells the reader what to look for. "Profit by State" tells them what is plotted, which they can already see.

---

## 3. Reference lines where zero means something (5 min)

On `Discount vs Margin` and `Profit by Sub-Category`:

> Right-click the axis ▸ **Add reference line** ▸ Value: **Constant, 0** ▸ Label: none ▸ Line: thin, grey.

On a chart with negative values, the zero line is the whole point — it separates "less good" from "actually losing money". Without it the reader has to trace the axis.

On `Pareto`, add a constant reference line at **0.8** on the cumulative axis, labelled 80%.

---

## 4. Colour discipline (5 min)

Three roles, and nothing else:

| Role | Where | Setting |
|---|---|---|
| **Diverging** | anything showing profit or margin | Red–Blue Diverging, **Centre = 0** |
| **Single accent** | sales bars, Pareto bars | one colour for every bar |
| **Grey** | moving average, context | `#767676` |

**Centre pinned at zero is not optional.** On automatic, Tableau spreads the palette between the minimum and maximum, so in a profitable period the least-profitable item still renders red and the chart lies.

**Red–Blue, not Red–Green.** Around 8% of men have red–green colour deficiency; for them a red–green profit map collapses into one flat field, losing exactly what the map exists to show. Red–blue keeps the warm/cool contrast under every type of CVD, and red still reads as alarm.

**One colour per bar chart.** Colouring each bar differently, or shading darker-where-bigger, encodes size twice — the bar length already says it — and burns the only free channel on information the chart already carries.

---

## 5. Viz in tooltip on the map (5 min, highest impact)

The single feature that turns the map from a picture into an instrument.

1. Open `Profit by State` ▸ Marks card ▸ **Tooltip**
2. Click **Insert ▸ Sheets ▸ `Profit by Sub-Category`**
3. Edit the inserted tag to `maxwidth="320" maxheight="260"`
4. Delete the rest of the default tooltip text except State, Sales, Profit

Now hovering a state answers "why is Texas red?" under the cursor, without a click and without leaving the page. Keep it under ~320×260 — a tooltip that fills the screen is a modal, and it covers the thing being pointed at.

---

## 6. Navigation between dashboards (3 min)

On each dashboard, drag a **Navigation** object from the Objects panel, set its target to the next dashboard, and label it. Four tabs with no visible way to move between them is a structure only the author knows about.

---

## 7. The closing sentence (2 min)

At the bottom of Painel 1, add a text object stating the finding:

> 10 of 49 states lose money. Orders discounted above 30% lose money 83% of the time — break-even sits at roughly 20%.

Most dashboards end with a chart and leave the conclusion to the reader. Stating it costs one text box and is the difference between a report and an argument.

---

## Before publishing

- [ ] Every bar chart is sorted
- [ ] Every profit/margin colour scale is centred on 0
- [ ] No dual axis with two different scales (synchronised, or derived, only)
- [ ] Filters apply to all worksheets using this data source
- [ ] Clicking a mark filters the page on every dashboard
- [ ] Titles read as findings, not field lists
- [ ] It reads correctly both filtered and unfiltered
- [ ] Save as **`.twbx`** — a `.twb` keeps its extract in a temp folder that Windows eventually clears
