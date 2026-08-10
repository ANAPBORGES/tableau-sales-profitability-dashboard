# Tableau Calculated Fields

All calculations for the Executive Sales & Profitability dashboard, in **Tableau syntax**.
Copy each into *Analysis ▸ Create Calculated Field* with the given name.

Names match the Power BI model measure-for-measure, so the same metric never carries
two names across the portfolio — see the dictionary at the bottom.

---

## Core profitability

**Profit Margin %**
```
SUM([Profit]) / SUM([Sales])
```
> Format as a percentage. This is the backbone metric — colour maps and tables by it.
> `SUM/SUM`, never `AVG` of a row-level ratio: an average of per-row margins weights a
> US$5 line the same as a US$5,000 one.

**Profit or Loss** (flag for colour)
```
IF SUM([Profit]) < 0 THEN "Loss" ELSE "Profit" END
```

**Avg Discount**
```
AVG([Discount])
```

**Total Orders** · **Total Customers**
```
COUNTD([Order ID])
COUNTD([Customer ID])
```

**Avg Order Value** · **Revenue per Customer**
```
SUM([Sales]) / COUNTD([Order ID])
SUM([Sales]) / COUNTD([Customer ID])
```

---

## Sizing the money at stake

A manager cannot act on "10 states lose money" — they act on what it costs. These four
turn the diagnosis into a number.

**Receita Entregue em Desconto** — revenue given up to discounting
```
SUM([Sales] * [Discount])
```

**Prejuízo dos Pedidos Deficitários** — what the loss-making orders cost
```
SUM(IF [Profit] < 0 THEN [Profit] END)
```

**% Pedidos no Prejuízo** — share of orders that lose money
```
COUNTD(IF [Profit] < 0 THEN [Order ID] END) / COUNTD([Order ID])
```
> Plotted against `Faixa de Desconto`, this is the evidence for the dashboard's closing
> claim. Without it the sentence asserts something no chart on the page shows.

**Faixa de Desconto** — discount band, numbered so it sorts by size, not alphabet
```
IF [Discount] = 0 THEN "1. Sem desconto"
ELSEIF [Discount] <= 0.1 THEN "2. Até 10%"
ELSEIF [Discount] <= 0.2 THEN "3. 11-20%"
ELSEIF [Discount] <= 0.3 THEN "4. 21-30%"
ELSE "5. Acima de 30%" END
```

**Days to Ship**
```
DATEDIFF('day', [Order Date], [Ship Date])
```

---

## Table calculations (trend & Pareto)

Each one needs **Compute Using** set on the sheet. Defined but not addressed, a table
calculation returns a plausible wrong number and reports no error.

**YoY Sales Growth** — compute along the date axis:
```
(SUM([Sales]) - LOOKUP(SUM([Sales]), -1)) / LOOKUP(SUM([Sales]), -1)
```
> Blank on the first year is correct — there is no prior year to compare against.

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
- `Profit Margin %`, `YoY Sales Growth`, `% Pedidos no Prejuízo` → Percentage (1 decimal).
- `Sales`, `Profit`, `Receita Entregue em Desconto`, `Prejuízo dos Pedidos Deficitários`
  → Currency ($, 0 decimals).
- Diverging colour (red ↔ blue) centred on **0** for anything showing profit or margin;
  a single colour for sales-only bars.

---

## Dictionary — same metric, same name in both tools

| This workbook | Power BI model | Definition |
|---|---|---|
| `Profit Margin %` | `Profit Margin %` | profit ÷ sales, aggregated |
| `Total Orders` | `Total Orders` | distinct count of order id |
| `Total Customers` | `Total Customers` | distinct count of customer id |
| `Avg Order Value` | `Avg Order Value` | sales ÷ orders |
| `Revenue per Customer` | `Revenue per Customer` | sales ÷ customers |
| `Avg Discount` | `Avg Discount` | simple average of discount |
| `Receita Entregue em Desconto` | `Revenue Lost to Discount` | Σ sales × discount |
| `% Pedidos no Prejuízo` | `Loss-Making Orders %` | loss-making orders ÷ orders |
| `Faixa de Desconto` | `discount_tier` | discount band (engineered in Power Query) |
| `Days to Ship` | `days_to_ship` | ship date − order date |

The Power BI side also carries a weighted discount (`Weighted Avg Discount`) and the
what-if simulation, which have no Tableau equivalent — that is deliberate: the
simulation belongs where the parameter lives.
