# Hyperliquid Bitcoin Fear & Greed Index Analysis

**Trader Behavior & Performance Analysis**
*How market sentiment influences trader behavior on Hyperliquid*

---

## Objective

Analyze whether **Bitcoin market sentiment** (Fear & Greed Index) explains differences in **Hyperliquid trader behavior and performance**, and produce **actionable, data-backed strategy rules**.

---

## Repository Structure

```
.
├── notebooks/
│   ├── 01_data_understanding.ipynb   # Dataset audit, viability check
│   ├── 02_data_extraction.ipynb      # Load, parse, merge, engineer metrics
│   ├── 03_data_cleaning.ipynb        # Remove noise, standardize, export
│   ├── 04_eda.ipynb                  # Distributions, trends, trader profiles
│   ├── 05_deep_analysis.ipynb        # Core analysis questions
│   ├── 06_validation.ipynb           # Cross-checks and sanity tests
│   ├── 07_insights.ipynb             # Insight cards
│   ├── 08_recommendations.ipynb      # Strategy rules with evidence
│   └── 09_visualization.ipynb        # Presentation-ready charts
│
├── charts/
│   ├── CHART_01_dashboard.png
│   ├── CHART_02_segment_sensitivity.png
│   ├── CHART_03_before_after.png
│   └── CHART_04_cumulative_pnl.png
│
└── README.md
```

---

## Setup

### Requirements

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
```

### Python Version

* Python **3.9+** recommended

---

## Data Setup

* `fear_greed_index.csv` — Bitcoin Fear & Greed Index
* `historical_data.csv` — Hyperliquid trade history

---

## How to Run

Run the notebooks **in order**, as each depends on the previous output:

```bash
jupyter notebook
```

Execution order:

```
01 → 02 → 03 → 04 → 05 → 06 → 07 → 08 → 09
```

---

## Data Pipeline

```
historical_data.csv + fear_greed_index.csv
        ↓ (02_data_extraction)
  working_trades.csv
        ↓ (03_data_cleaning)
  clean_trades.csv + daily_per_trader.csv
        ↓ (04 → 09)
  charts, insights, model outputs
```

---

## Datasets

| Dataset                    | Rows                          | Columns | Period              |
| -------------------------- | ----------------------------- | ------- | ------------------- |
| Bitcoin Fear & Greed Index | 2,644                         | 4       | Feb 2018 – May 2025 |
| Hyperliquid Trade Records  | 211,224 (raw) → 211,068 clean | 16 → 22 | May 2023 – May 2025 |

---

## Outputs

*  Sentiment-driven performance insights
*  Trader segmentation & behavior patterns
*  Before/after sentiment impact analysis
*  Actionable trading strategy rules
*  Presentation-ready visualizations

---

## Key Questions Answered

1. Does market sentiment affect trader profitability?
2. Do different trader segments respond differently to sentiment?
3. Can sentiment be used as a predictive signal for strategy design?

---

## Final Deliverable

A **data-driven framework** for incorporating Bitcoin sentiment into trading strategies on Hyperliquid.

