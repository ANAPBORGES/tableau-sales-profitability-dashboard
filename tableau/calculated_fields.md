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

## Suggested formatting
- `Profit Ratio`, `YoY Sales Growth` → Percentage (1 decimal).
- `Sales`, `Profit` → Currency ($, 0 decimals).
- Diverging colour (red ↔ green) centred on 0 for `Profit`; sequential for `Sales`.
