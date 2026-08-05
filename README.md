# Tableau — Executive Sales & Profitability Dashboard
> An interactive **Tableau** dashboard on the Superstore dataset, focused on *where the business makes money and where it loses it* — filled profit maps, category profitability, a sub-category Pareto and a discount scatter. Built to Tableau's strengths (mapping, interactivity, visual comparison), and fully specced for reproduction.

[![Tableau](https://img.shields.io/badge/Tableau-Public-E97627?style=flat&logo=tableau&logoColor=white)](https://public.tableau.com/)
[![Data](https://img.shields.io/badge/Data-included%20in%20repo-34A853?style=flat)](./data/superstore.csv)
[![Status](https://img.shields.io/badge/Status-Spec%20complete%20·%20publishing-yellow?style=flat)]()

<!-- After publishing, replace the line below with your live link:
[![Live Dashboard](https://img.shields.io/badge/Live-Tableau%20Public-E97627?style=flat&logo=tableau)](YOUR_TABLEAU_PUBLIC_URL) -->

---

## Business Context

**Industry:** Retail
**Stakeholders:** Commercial, Finance, and executive leadership
**Business question:** *Which regions, categories and customers are actually profitable, and how much profit is discounting costing us?*

Where the [Power BI project](https://github.com/ANAPBORGES/saas-financial-kpis) frames the Superstore data as **financial KPIs** (MRR, churn), this Tableau dashboard takes the **executive sales & profitability** angle — geography, product mix, and a hands-on discount lever — leveraging what Tableau does best: mapping, interactivity, and Level-of-Detail analysis.

---

## Dataset

| Field | Details |
|---|---|
| **Source** | Sample Superstore — included at [`data/superstore.csv`](./data/superstore.csv) |
| **Size** | 9,994 order lines · 5,009 orders · 793 customers |
| **Period** | 2015 – 2018 · US$2.30M sales · US$286K profit (12.5% margin) |

Self-contained: the CSV lives in the repo, so the dashboard is reproducible with no external download.

---

## What's in this repo

| File | Purpose |
|---|---|
| [`tableau/dashboard_design.md`](./tableau/dashboard_design.md) | **The design** — layout grid, interaction model, colour roles, type scale, and the reasoning behind each |
| [`tableau/finishing_guide.md`](./tableau/finishing_guide.md) | The last 30 minutes — sorting, titles, reference lines, viz-in-tooltip, colour discipline |
| [`tableau/dashboard_spec.md`](./tableau/dashboard_spec.md) | Sheet-by-sheet spec, dashboard layout, filters and interactions |
| [`tableau/workbook/`](./tableau/workbook/) | **The Tableau workbook (.twb)** — data source typed, geographic roles assigned, all calculated fields defined |
| [`tableau/calculated_fields.md`](./tableau/calculated_fields.md) | Every calculated field in Tableau syntax — profitability metrics and table calculations |
| [`tableau/build_guide.md`](./tableau/build_guide.md) | Step-by-step to build and publish it on Tableau Public (free) |
| [`data/superstore.csv`](./data/superstore.csv) | The dataset |

---

## Tableau techniques demonstrated

- **Filled maps** with a diverging profit palette centred on zero — loss states jump out.
- **Table calculations** — running totals, cumulative % for Pareto, moving averages, YoY growth.
- **Dashboard actions** — click a state to filter every view; global Region/Category/Year filters.

---

## Key insights (validated against the data)

1. **10 of 49 states lose money** — *Texas* (−US$26K) and *Pennsylvania* (−US$16K) are red on the map despite high sales; California (+US$76K) and New York (+US$74K) carry the business.
2. **Furniture is a revenue trap** — it sells almost as much as Technology but returns just a **2.5% profit ratio** (vs 17% for Technology and Office Supplies); *Tables* and *Bookcases* are outright loss-makers.
3. **Discounting is the profit lever** — profit turns negative **above ~20% discount**; orders over 30% discount have an **83% loss rate**.
4. **Concentration** — the top 6 of 17 sub-categories drive ≈ **65% of sales** (Pareto).

---

## Live dashboard

🔗 **Tableau Public:** _publishing in progress_ — see [`build_guide.md`](./tableau/build_guide.md).
Screenshots will be added to [`assets/`](./assets/) once published.

---

## About

Built by **Ana Paula Borges** · [LinkedIn](https://linkedin.com/in/ana-paula-d-araújo-borges) · [GitHub](https://github.com/ANAPBORGES)

*Senior Data Analyst & Team Leader with 10+ years in BI, DataViz, and Marketing Analytics.*
