# Customer Lifetime Value & Cohort Analysis

End-to-end CLV analysis on **~8,000 retail transactions (2011–2014)** — cleaning, EDA, regression, and a full cohort-based Historic CLV pipeline that revealed the average 2011-acquired customer is worth **~$1,600** over 48 months.

---

## Business Problem

A retailer wants to know how much its customers are worth over their lifetime, whether discounts actually drive profit, and which factors move sales. Answers feed directly into how much the business can afford to spend acquiring new customers.

## Data

- **Source:** Retail transactional data (`customer_sales.csv`)
- **Period:** Jan 2011 – Sep 2014 (~3.5 years)
- **Scale:** 4,116 unique orders, 792 unique customers across multiple US regions
- **Target:** Sales, Profit, and cumulative spend by cohort

## Methodology

**Data preparation**
- Parsed accounting-format currency strings (e.g., `"(500)"` → `-500`) to float
- Converted date strings to datetime for cohort arithmetic
- IQR-based outlier removal on Sales (1.5× IQR bounds)
- Feature engineering: `Year`, `Age` (months since acquisition), `AgeRange` buckets

**Analysis**
- Top-state-by-year revenue ranking via `groupby().head(3)` pattern
- Discount vs. Profit curve analysis to find the profit-maximizing discount level
- OLS regression (statsmodels) with one-hot-encoded categoricals (Segment, Category, Region, Ship Mode)
- Cohort analysis: assign `OriginYear` per customer → pivot by `(OriginYear, AgeRange)` → cumulative sum → divide by cohort size = Historic CLV

## Key Findings

| Question | Finding |
|---|---|
| Do sales grow over time? | **No** — flat 2011–2014, no upward trend |
| Optimal discount level? | **0%** — every discount above 0% reduces profit; at 40–50% discount, profit turns negative |
| Top Sales drivers (OLS, p < 0.05) | Quantity (+), Profit (+), Technology category (+), Region North (−) |
| 2011 cohort cumulative CLV at 48 months | **~$1,600 per customer** |
| Newer cohorts vs 2011? | Higher early-age CLV — suggests improving customer quality over time |

## Tech Stack

`Python 3` · `pandas` · `numpy` · `matplotlib` · `seaborn` · `statsmodels` · `ydata-profiling` · `Jupyter`

## Repository Structure

```
├── notebooks/
│   └── clv_analysis.ipynb       # Full end-to-end analysis
├── data/
│   └── customer_sales.csv       # Input dataset
├── visualizations/
│   ├── cohort_clv_curves.png
│   ├── discount_vs_profit.png
│   └── sales_over_time.png
├── requirements.txt
└── README.md
```

## How to Reproduce

```bash
git clone https://github.com/JazimMehmoodQureshi/customer-lifetime-value-cohort-analysis.git
cd customer-lifetime-value-cohort-analysis
pip install -r requirements.txt
jupyter notebook notebooks/clv_analysis.ipynb
```

## Business Impact

- Sets a **$1,600 ceiling on Customer Acquisition Cost** (CAC) — enables CAC:CLV ratio tracking
- Debunks the "discounts drive volume" assumption with transaction-level evidence
- Provides a **reusable cohort-analysis template** the business can re-run quarterly

## Author

**Jazim Mehmood Qureshi** · BSc (Hons) Management Science, LUMS · [LinkedIn](https://linkedin.com/in/jazimmehmoodqureshi)