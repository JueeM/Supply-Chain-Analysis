# Supply-Chain-Analysis
Root-cause analysis of FMCG SKU stockouts using statistical hypothesis testing (t-tests, bootstrap resampling, logistic regression). Diagnosed a €9,601 inventory risk, validated a €104 safety stock fix via counterfactual simulation, and built demand forecasting, EOQ/ROP, and Power BI dashboards to support inventory decision-making.

# Stockout Root-Cause Analysis & Inventory Optimization

A supply chain analytics project diagnosing the root cause of recurring stockouts for a high-risk FMCG SKU, quantifying the financial impact, and validating a data-driven inventory fix — using hypothesis-driven statistical testing rather than assumption.

## Overview

Using a simulated multi-country FMCG sales dataset (used as a proxy for real operational data, since proprietary supply chain data isn't publicly available), this project:

1. Benchmarks 3 beverage SKUs in the German market by a composite risk score (stockout rate, lead time, sales volume)
2. Systematically tests 5 candidate root causes for the highest-risk SKU's stockouts, ruling out 4 and isolating one statistically significant driver
3. Quantifies the financial impact with uncertainty bounds (bootstrap confidence interval)
4. Validates the proposed fix via counterfactual simulation
5. Extends into demand forecasting, EOQ/reorder point calculation, and a Power BI dashboard

**The methodology — not the specific dataset — is the transferable part.** The same hypothesis-elimination, statistical validation, and uncertainty-quantification approach applies directly to real supply chain data.

## Key Findings

| Metric | Result |
|---|---|
| SKU analyzed | SKU0013 (BrandA Water), Germany |
| Stockout rate | 3.6% |
| Root cause | Under-sized safety stock (t = -2.07, **p = 0.045**) |
| Ruled out | Supplier concentration, lead time, promotions, seasonality, calendar effects |
| Estimated lost margin | **€9,601** (95% CI: €8,688–€10,381, via bootstrap resampling) |
| Recommended fix | +24 units safety stock (~€104 one-time cost) |
| Estimated ROI | ~92x |
| Demand forecast accuracy | 19.2% MAPE (6-week rolling average) |
| EOQ / Reorder Point | 2,248 units / 835 units |

Full methodology, statistical tests, and limitations are documented in [`SKU0013_Stockout_Analysis_Report.md`](./SKU0013_Stockout_Analysis_Report.md).

## Methodology

Rather than assuming a root cause, five hypotheses were tested independently using appropriate statistical methods, with explicit correction for multiple comparisons where relevant:

- **Demand seasonality** → correlation analysis → ruled out
- **Supplier concentration** → z-score testing across 60+ suppliers, corrected for multiple comparisons → ruled out
- **Lead time** → correlation + bucketed comparison → ruled out
- **Promotions/discounting** → group comparison → ruled out
- **Weekend/holiday timing** → group comparison → ruled out
- **Safety stock sizing** → independent t-test → **supported**

The finding was then:
- **Cross-validated** against 2 benchmark SKUs to confirm it was SKU-specific, not category-wide
- **Stress-tested** with a multivariate logistic regression (reported transparently as underpowered given the small sample, n=39 stockout events)
- **Financially quantified** with bootstrap resampling to produce a confidence interval, not just a point estimate
- **Validated** via counterfactual simulation, with an explicit caveat around demand censoring (true demand on stockout days can't be directly observed)

## Repository Contents

```
├── SKU0013_Stockout_Analysis_Report.md   # Full write-up: methodology, findings, limitations
├── notebook.ipynb                         # Complete analysis code
├── powerbi_exports/                       # CSVs feeding the Power BI dashboard
├── dashboard_screenshot.png               # Power BI dashboard preview
└── README.md
```

## Tools

Python (Pandas, SciPy, Statsmodels, Matplotlib), Power BI

## Limitations

This project is transparent about its constraints rather than overstating confidence:
- Small sample size (39 stockout events) limits precision of financial estimates — addressed via bootstrap confidence intervals rather than a single point figure
- Uses a simulated dataset as a proxy for real operational data
- The counterfactual simulation's ~95% prevention estimate is an optimistic upper bound due to demand censoring (explained in the full report)
- EOQ/Reorder Point figures use standard textbook cost assumptions, not data-derived costs, and a documented mismatch with observed stock levels is discussed in the report

Full details in the [analysis report](./SKU0013_Stockout_Analysis_Report.md).

## Author

Juee Satish Mahajan
