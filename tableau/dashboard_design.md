# Dashboard design — Executive Sales & Profitability

The build spec for the dashboard, at the level of detail that decides whether it reads as competent or as senior. [`dashboard_spec.md`](./dashboard_spec.md) lists *what sheets exist*; this file is about **structure, interaction and hierarchy** — the part that is design rather than assembly.

---

## The argument the dashboard makes

A dashboard without a thesis is a pile of charts. This one says:

> **We are growing, and growth is not profit.** Sales rise steadily, but a third of the states and several whole sub-categories lose money — and the cause is discounting, which is concentrated rather than spread.

Every element earns its place by advancing that sentence. Anything that does not is cut. That is the first design decision, and it is made before any chart.

---

## Layout — 1200 × 900

```
┌───────────────────────────────────────────────────────────────────────┐
│ Executive Sales & Profitability                        Superstore     │  56
│ 2015–2018 · showing: All regions, All categories                      │  ← dynamic
├───────────────────────────────────────────────────────────────────────┤
│  [ Region ▾ ]   [ Category ▾ ]   [ Year ▾ ]         [ Top N: ——●—— ]  │  48
├──────────────┬──────────────┬──────────────┬──────────────────────────┤
│ SALES        │ PROFIT       │ MARGIN       │ ORDERS                   │
│ $2,297,201   │ $286,397     │ 12.5%        │ 5,009                    │ 130
│ ▲ 20.4% YoY  │ ▲ 14.5% YoY  │ ▼ 0.4pp YoY  │ ▲ 18.9% YoY              │
│ ▁▂▃▅▆█       │ ▁▃▂▅▄█       │ ▅▄▅▃▄▃       │ ▁▂▄▅▆█                   │  ← sparklines
├──────────────────────────────────┬────────────────────────────────────┤
│                                  │                                    │
│   PROFIT BY STATE                │   WHERE THE MARGIN GOES            │
│   (filled map, diverging at 0)   │   Category › Sub-Category          │
│   viz-in-tooltip on hover        │   highlight table                  │ 340
│                                  │                                    │
├──────────────────────────────────┼────────────────────────────────────┤
│   SALES TREND                    │   DISCOUNT vs PROFIT               │
│   bars + 3-mo average            │   scatter, ref line at 0           │ 280
│                                  │                                    │
├───────────────────────────────────────────────────────────────────────┤
│ 10 of 49 states lose money. Orders discounted above 30% lose money     │  46
│ 83% of the time — break-even sits at roughly 20%.                     │  ← the "so what"
└───────────────────────────────────────────────────────────────────────┘
```

### Why this arrangement

**Top-left carries the most weight.** Western reading order means the eye lands there first and leaves bottom-right last. So: headline numbers at the top, the map (the "where") next, and the discount scatter — the *cause* — last, where someone who has read that far is ready for it.

**Four rows, not a grid of eight tiles.** Each row answers one question: *how big · where · when · why*. A reader can stop after any row with a complete thought.

**A closing sentence.** Most dashboards end with a chart and leave the conclusion to the reader. Stating it costs 46 pixels and is the difference between a report and an argument. Write it as the finding, not as a caption.

---

## The four moves that separate this from a basic build

### 1. KPI band with context, not bare numbers

A number alone is not information — `$2,297,201` answers nothing without a comparison. Each tile carries three layers:

| Layer | What it adds |
|---|---|
| The value | magnitude |
| YoY delta with arrow | direction, vs the only comparison anyone asks for |
| Sparkline | shape — steady growth vs a spike vs a decline |

**Build:** each KPI is *two* worksheets stacked in a vertical container — a text sheet (BAN + delta) and a 40px-tall line sheet with axes hidden, no labels, no gridlines.

```
Sales YoY %  =  (SUM([Sales]) - LOOKUP(SUM([Sales]), -1)) / LOOKUP(SUM([Sales]), -1)

KPI Sales Label =
  "▲ " if [Sales YoY %] >= 0 else "▼ "  →  written as:
  IF [Sales YoY %] >= 0 THEN "▲ " ELSE "▼ " END
    + STR(ROUND(ABS([Sales YoY %]) * 100, 1)) + "% YoY"
```

Colour the arrow by sign — and **only the arrow**, not the number. A green `$2,297,201` reads as "this figure is good", which is a claim the number itself is not making.

### 2. Viz in tooltip on the map

Hovering a state pops a **miniature bar chart** of that state's profit by sub-category, inside the tooltip.

This is the single highest-impact feature in the build. It turns the map from a picture into an instrument: the reader asks "why is Texas red?" and the answer appears under the cursor, without a click, without leaving the page.

**Build:**
1. Make a worksheet `TT State SubCategory` — `Sub-Category` on Rows, `Profit` on Columns, bars, coloured by `Profit or Loss`, sorted ascending, filter to top/bottom 8
2. On the map sheet, open **Dica de ferramenta** and insert:
   ```
   <Sheet name="TT State SubCategory" maxwidth="320" maxheight="260" filter="<All Fields>">
   ```
3. Strip the tooltip of everything else — title, axis, legend

Keep the tooltip under ~320×260. A tooltip that fills the screen is a modal, and it will cover the thing the reader was pointing at.

### 3. Real interactivity — four actions, each with a job

| Interaction | Type | What it is for |
|---|---|---|
| Click a state | **Filter** → all sheets | "Show me this market" |
| Click a sub-category | **Highlight** → trend + scatter | Trace one line without losing context |
| `Top N` slider | **Parameter** → Pareto | Reader controls the depth of the tail |
| Hover a state | **Viz in tooltip** | Answers "why" without navigation |

**Filter versus highlight is a real decision, not a preference.** Filtering removes data — good for "focus on this", bad when the reader needs to know how the part compares to the whole. Highlight keeps everything on screen and dims the rest. The map filters because a region genuinely is a separate market; the category table highlights because a sub-category only means something against the others.

**Top N parameter** (integer, 3–17, step 1) driving the Pareto:

```
Top N Filter  =  INDEX() <= [p_TopN]
```

placed on Filters, set to True, computed along Sub-Category.

### 4. The dynamic subtitle

Under the title, a text sheet reporting what the reader is currently looking at:

```
Dashboard Subtitle =
  "2015–2018 · showing: "
  + IF ISNULL([Region]) THEN "All regions" ELSE ATTR([Region]) END
  + ", " + ...
```

Small, grey, one line. It removes the most common dashboard failure: a filtered screenshot pasted into a deck with nobody remembering it was filtered.

---

## Colour — three roles, no more

| Role | Where | Rule |
|---|---|---|
| **Diverging** (red ↔ blue, grey at 0) | map, highlight table, scatter | Centre pinned at **0**, always |
| **Single accent** | sales bars, Pareto bars | One colour. Bar length already encodes size |
| **Grey** | moving average, context, gridlines, all text | Everything that is not the subject |

**Red–blue, not red–green.** Around 8% of men have red–green colour deficiency, and for them a red–green profit map turns into one flat yellow-grey field — losing precisely the thing the map exists to show. Red–blue keeps the warm/cool contrast under every CVD type, and red still reads as alarm.

**The centre pinned at 0 is not cosmetic.** Left on automatic, Tableau spreads the palette between the minimum and maximum, so in a profitable quarter the least-profitable state still renders red. Pinned at zero, colour becomes absolute: red *means* loss.

**Text never wears a series colour.** Values, labels and titles stay in grey ink; the coloured mark beside them carries identity. Coloured numbers read as editorial.

---

## Type and spacing

| Element | Size | Weight | Colour |
|---|---|---|---|
| Dashboard title | 20 | Bold | #1A1A1A |
| Subtitle | 11 | Regular | #767676 |
| Sheet titles | 12 | Bold | #333333 |
| KPI value | 28 | Regular | #1A1A1A |
| KPI delta | 11 | Regular | by sign |
| Axis / labels | 9–10 | Regular | #767676 |

**Four sizes, two weights, three greys.** Every extra step is noise. Type hierarchy is what makes a dashboard feel designed, and it costs nothing but consistency.

**Padding: 8px inner on every object, 16px outer on the dashboard.** Use **layout containers**, not free-floating objects — floating survives exactly until someone opens it on a different screen.

---

## What is deliberately not here

- **No pie charts.** 17 sub-categories in a pie is unreadable; the Pareto answers the same question with an order.
- **No dual axis with two different scales.** The trend's second axis is *synchronised* (same unit); the Pareto's cumulative line is *derived from* its bars. Those are the only two legitimate cases. `Profit Ratio` against `Sales` on twin axes invents a correlation the data does not contain.
- **No gauges, no donuts, no 3D.** They cost space and give back less than a number.
- **No eighth colour.** Past seven classes, adjacent hues stop being distinguishable — fold the tail into "Other" or use a table.

---

## Build order

Build in this order; each step is checkable on its own.

1. `Sales YoY %`, `Profit YoY %`, `Orders YoY %`, plus the three label fields
2. Four KPI text sheets + four sparkline sheets
3. `TT State SubCategory` (the tooltip sheet)
4. Map + viz-in-tooltip
5. Highlight table
6. Trend (synchronised dual axis)
7. Scatter + reference line at 0
8. `p_TopN` parameter + `Top N Filter` → Pareto
9. Dashboard: containers, padding, type scale
10. Actions: filter, highlight, parameter
11. Dynamic subtitle + closing sentence
12. Test with a filter applied, then with none — both must read correctly
