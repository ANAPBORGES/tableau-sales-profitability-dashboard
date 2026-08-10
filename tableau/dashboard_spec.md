# Dashboard specification — as built

Five dashboards on 20 worksheets, telling one story:
**where we make money, where we lose it, and what discounting does to profit.**

Data: [`data/superstore.csv`](../data/superstore.csv) · Sample Superstore, 2015–2018,
US$2,297,201 sales · US$286,397 profit · 12.47% margin.

Metric names match the [Power BI project](https://github.com/ANAPBORGES/saas-financial-kpis)
field for field — see the dictionary in [`calculated_fields.md`](./calculated_fields.md).
The same measure never carries two names across the portfolio.

---

## Home

A cover page: the mark, the argument the dashboard makes, and the four destinations.
Navigation is through the workbook tabs.

---

## Executive Summary — *how much, where, and why*

| Worksheet | Chart | What it carries |
|---|---|---|
| `KPIs` | 4 BANs | Sales · Profit · `Profit Margin %` · `Total Orders` |
| `Money at Stake` | 2 BANs | `Revenue Lost to Discount` · `Loss from Unprofitable Orders` |
| `Profit by State` | filled map | `SUM(Profit)`, red–blue diverging **centred on 0** |
| `Category Profitability` | highlight table | Category › Sub-Category, text = Sales, colour = margin |
| `Yearly Growth` | bars + label | Sales by year with `YoY Sales Growth` on the label |
| `Discount vs Margin` | scatter | `AVG(Discount)` × `Profit Margin %`, one mark per sub-category, reference line at 0 |

Global filters — Region, Category, Year — apply to **every worksheet on the data source**,
so they are shown once, here. A closing line states the finding rather than leaving the
reader to infer it.

**Why the money tiles.** "10 states lose money" does not move anyone; what those states cost
does. `Money at Stake` is the difference between a diagnosis and a decision.

---

## Commercial Analysis — *margin, mix, and concentration*

| Worksheet | Chart | What it carries |
|---|---|---|
| `Margin Trend` | line | `Profit Margin %` by month, with an average reference line |
| `Sales Trend` | bars + line | monthly sales with the 3-month moving average |
| `Pareto` | bars + cumulative | sub-categories sorted **descending**, cumulative % line, reference at 80% |
| `Region x Category` | heat table | 12 cells: sales as text, margin as colour |
| `Profit by Sub-Category` | bars | sorted **ascending** — the loss-makers land on top, where the eye starts |
| `Segment Performance` | bars + label | sales by segment with margin as the label |

Executives read years; analysts read months. That is why the yearly comparison sits on the
first page and the monthly detail sits here, next to margin over time.

---

## Customers — *who buys, and who loses money*

| Worksheet | Chart | What it carries |
|---|---|---|
| `Customers by Segment` | bars | `Total Customers` with `Avg Order Value` on the label |
| `Margin by Segment and Region` | heat table | margin across the 3 × 4 grid |
| `Revenue per Customer by Region` | bars | `Revenue per Customer`, customer count on the label |
| `Top Customers` | bars | every account ranked by sales, coloured by `Profit or Loss` |

A segment is not something anyone can act on; an account is. `Top Customers` is the level
where a commercial decision actually happens — and a negative margin on a large account is
the first case worth reviewing.

---

## Price & Discount — *where discounting turns into loss*

| Worksheet | Chart | What it carries |
|---|---|---|
| `Loss Rate by Discount Band` | bars | `Loss-Making Orders %` by `Discount Band` — full width, leading the page |
| `Ship Mode Performance` | bars | sales by ship mode, coloured by margin |
| `Avg Days to Ship` | bars | actual lead time per mode, ascending |
| `Discount by Category` | bars | `AVG(Discount)` by category, coloured by margin |

`Discount Band` is numbered (`1. No discount` … `5. Over 30%`) so it sorts by size rather
than alphabetically. This page carries the evidence for the closing claim on page one: the
83% loss rate above 30% off is a chart here, not an assertion.

**What this page is not.** It was "Operations & Price" and the operations half did not hold
up: Superstore has no freight cost, so delivery cannot be judged on profitability, and
"First Class ships faster than Standard" is a tautology, not a finding. Delivery stayed, in
third position; the page is now about price.

---

## Layout

All five dashboards are **1200 × 1000, fixed**, and share one frame:

```
┌──────────────────────────────────────────────────────────┐
│ ▪ SUPERSTORE   Home · Executive Summary · … · Price      │  56 px header image
├──────────────────────────────────────────────────────────┤
│ [ Region ▾ ]  [ Category ▾ ]  [ Year ▾ ]   [ legend ]    │  filters, page 1/3/4
├──────────────────────────────────────────────────────────┤
│  content, in white cards on a #F2F4F8 ground             │
├──────────────────────────────────────────────────────────┤
│  the finding · then source, period and scope             │
└──────────────────────────────────────────────────────────┘
```

The header is an image per dashboard, with **the current tab in white and the others
muted** — a menu where every item has the same weight tells the reader nothing about where
they are. Ground `#F2F4F8`, cards white with a `#E3E5E8` hairline: the same values as the
Power BI report, so the two read as one portfolio.

---

## Interactions

| Interaction | Type | Why that type |
|---|---|---|
| Click a state | **Filter** → the page | A region genuinely is a separate market |
| Click a cell in `Region x Category` | **Filter** → the page | Same |
| Click a segment × region cell | **Filter** → the page | Same |
| Click a ship mode | **Filter** → the page | Same |
| Region / Category / Year | Global filters on the data source | Set once, apply everywhere |

Filtering removes data — right for "focus on this market", wrong when the reader needs the
part against the whole.

---

## Colour

Three roles, and nothing else:

| Role | Where | Setting |
|---|---|---|
| **Diverging** | anything showing profit or margin | Red–Blue, **centre = 0** |
| **Single accent** | sales bars, Pareto bars | one colour for every bar |
| **Grey** | moving average, context, all text | `#767676` |

Centre pinned at zero is not cosmetic: left automatic, Tableau spreads the palette between
the minimum and the maximum, so in a profitable period the least-profitable item still
renders red and the chart lies. Red–blue rather than red–green keeps the warm/cool contrast
under every type of colour vision deficiency.

---

## Publishing

The workbook carries a data extract and five layout images, and a `.twb` packages
**neither** — it stores paths, and Tableau puts the extract in the Windows temp folder.
Save as **`.twbx`** before publishing or archiving, or the extract disappears and the header
images break for anyone who opens it.
