# Tableau Calculated Fields

All calculations for the Executive Sales & Profitability dashboard, in **Tableau syntax**.
Copy each into *Analysis ▸ Create Calculated Field* with the given name.

---

## Core profitability

**Profit Ratio**
```
SUM([Profit]) / SUM([Sales])
```
> Format as a percentage. This is the backbone metric — colour maps and tables by it.

**Profit or Loss** (flag for colour)
```
IF SUM([Profit]) < 0 THEN "Loss" ELSE "Profit" END
```

**Avg Discount**
```
AVG([Discount])
```

---

## LOD expressions (Level of Detail)
Tableau's headline feature — aggregate at a grain independent of the view.

**Customer Sales (lifetime)** — fixes total sales per customer regardless of the current view:
```
{ FIXED [Customer ID] : SUM([Sales]) }
```

**Customer Profit (lifetime)**
```
{ FIXED [Customer ID] : SUM([Profit]) }
```

**Customer Value Tier** — bucket customers by lifetime value:
```
IF   [Customer Sales (lifetime)] >= 5000 THEN "1 - High (>= $5K)"
ELSEIF [Customer Sales (lifetime)] >= 1000 THEN "2 - Mid ($1K-$5K)"
ELSE "3 - Low (< $1K)"
END
```

**Sales vs Region Average** — compare a state to its region without losing the state grain:
```
SUM([Sales]) - { EXCLUDE [State] : AVG( { FIXED [State] : SUM([Sales]) } ) }
```

**% of National Sales** — each state's share, computed with an EXCLUDE LOD:
```
SUM([Sales]) / { EXCLUDE [State] : SUM([Sales]) }
```

---

## Table calculations (trend & Pareto)

**YoY Sales Growth** — compute along the date axis:
```
(SUM([Sales]) - LOOKUP(SUM([Sales]), -1)) / LOOKUP(SUM([Sales]), -1)
```

**Running Total of Sales**
```
RUNNING_SUM(SUM([Sales]))
```

**Cumulative % of Sales (Pareto)** — sort sub-categories descending, then:
```
RUNNING_SUM(SUM([Sales])) / TOTAL(SUM([Sales]))
```

**3-Month Moving Average**
```
WINDOW_AVG(SUM([Sales]), -2, 0)
```

---

## What-if: discount simulation
Shows what profit *would* be at a chosen discount level — an interactive lever for stakeholders.

**Parameter:** `What-if Discount` · Float · range 0.0–0.8, step 0.05, current 0.20.

**Simulated Profit** — approximate margin impact of moving every order to the chosen discount.
Cost is inferred from the actual profit at the actual discount, then re-priced:
```
// Actual unit economics
[Actual Revenue]  = SUM([Sales])
[Actual Cost]     = SUM([Sales]) - SUM([Profit])
// List price backed out of the current discounted revenue
[List Revenue]    = SUM([Sales]) / (1 - AVG([Discount]))
// Re-price at the what-if discount, cost unchanged
[Simulated Revenue] = [List Revenue] * (1 - [What-if Discount])
[Simulated Profit]  = [Simulated Revenue] - [Actual Cost]
```
> Build these as chained calculated fields. Plot **Simulated Profit** against the parameter to
> show the break-even discount (~20% in this dataset).

---

## Suggested formatting
- `Profit Ratio`, `% of National Sales`, `YoY Sales Growth` → Percentage (1 decimal).
- `Sales`, `Profit`, `Simulated Profit` → Currency ($, 0 decimals).
- Diverging colour (red ↔ green) centred on 0 for `Profit`; sequential for `Sales`.
