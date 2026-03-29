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

---

## Write-Up

### Methodology

**Data preparation (Notebooks 01–03)**

The Bitcoin Fear/Greed Index provides a daily sentiment score (0–100) bucketed into five classes: Extreme Fear, Fear, Neutral, Greed, Extreme Greed. The Hyperliquid dataset covers 211k trade-level records across 32 accounts over two years. The two tables were joined on calendar date at the daily level.

Six metrics were engineered at the trade level: net PnL (Closed PnL minus Fee), closing-trade flag, win flag (PnL > 0 on a close), direction category, leverage proxy (Size USD / |Start Position|, capped at 100×), and trade size bucket. A separate daily-per-account aggregation was exported for time-series analysis.

**Analysis approach (Notebooks 04–08)**

All sentiment comparisons use **median** rather than mean — PnL distributions are heavily right-skewed (range: −$117,990 to +$135,329 per trade). Statistical significance is tested with the **Mann-Whitney U test** (non-parametric, appropriate for skewed financial data).

Three trader segments were defined: High vs Low Leverage (split at median 0.20×, 16 traders each), Frequent vs Infrequent (split at median 3,698 trades, 16 traders each), and Consistent Winners vs Inconsistent (split at 50% win rate).

---

### Insights

#### Insight 1 — Fear days dramatically outperform Greed days

Median **daily PnL on Fear days = $13,527** vs **Greed days = $1,151** — an 11.7× difference, statistically significant (Mann-Whitney U, p < 0.0001). Daily win rate: Fear 96.5% vs Greed 95.5%.

At the individual trade level, the same pattern holds across all five sentiment buckets:

| Sentiment | Median PnL / trade | Win rate |
|---|---|---|
| Extreme Fear | $8.05 | 80.0% |
| Fear | $7.11 | **88.6%** ← highest |
| Neutral | $4.40 | 83.0% |
| Greed | $3.98 ← lowest | 76.1% ← lowest |
| Extreme Greed | $7.13 | 87.4% |

Extreme Greed also produces the worst average drawdown (−$32,694), while Extreme Fear produces the best (−$4,885). The result is counter-intuitive and explained by Insight 2.

---

#### Insight 2 — These traders are contrarian: long on Fear, short on Greed

The Long/Short open ratio reveals the trading posture:

| Sentiment | L/S Ratio | Posture |
|---|---|---|
| Extreme Fear | **2.21×** | Strongly net long |
| Fear | 1.64× | Net long |
| Neutral | 1.61× | Net long |
| Greed | **0.73×** | Net SHORT |
| Extreme Greed | **0.82×** | Net SHORT |

On Greed and Extreme Greed days these traders are net short — they fade the market. Trade size confirms the same direction: Extreme Fear median size ($767) is **53% larger** than Extreme Greed ($500). Daily trade frequency is 6× higher on Extreme Fear (1,528 trades/day) vs Greed (260 trades/day).

Binary Fear vs Greed behavior and performance summary:

| Metric | Fear days | Greed days |
|---|---|---|
| Median PnL / trade | $7.32 | $5.19 |
| Mean PnL / trade | $118.28 | $59.66 |
| Win rate | 86.3% | 80.8% |
| Median trade size | $749.67 | $552.85 |
| Median leverage proxy | 0.17× | 0.18× (not significant, p=0.051) |
| L/S ratio | 1.64× (net long) | 0.73× (net short) |

Trade size difference between Fear and Greed is statistically significant (p < 0.0001). Leverage is not (p = 0.051).

---

#### Insight 3 — Strategy type determines sentiment sensitivity; leverage does not

Frequent and infrequent traders show near-inverted win rate profiles across sentiment regimes:

| Sentiment | Frequent WR | Infrequent WR |
|---|---|---|
| Extreme Fear | 83.9% | 67.8% |
| Fear | **89.1%** | 83.6% |
| Neutral | 82.9% | 83.8% |
| Greed | 74.5% | **91.8%** |
| Extreme Greed | 87.4% | 87.6% |

Frequent traders outperform by +5.5pp on Fear; infrequent traders outperform by +17.3pp on Greed. These are different strategy archetypes — systematic/contrarian vs momentum — each optimised for a different sentiment regime.

High-leverage traders do not outperform low-leverage traders. Low-leverage traders have higher median PnL across most regimes: most starkly on Extreme Greed, where low-leverage ($13.26) outperforms high-leverage ($3.28) by 4×. Leverage is not the differentiating variable here.

---

#### Insight 4 — Nearly all traders are consistent winners; the sample has selection bias

All 32 traders have win rates above 71% (range: 71.4%–100%). The Consistent Winner vs Inconsistent segmentation is degenerate — there are effectively no struggling traders in this dataset. This is an important limitation: findings reflect a self-selected group of successful accounts and **should not be generalised** to retail traders without validation on a broader dataset.

---

### Strategy Recommendations

#### Rule 1 — Lean into Fear; reduce long exposure on Extreme Greed

*Applies to: systematic/contrarian traders*

Treat Extreme Fear readings as high-conviction entry signals. Increase position size and trade frequency. On Extreme Greed, reduce long exposure and consider short bias — the profitable traders in this dataset are already doing exactly this.

**Evidence:**
- Fear daily PnL is 11.7× Greed (p < 0.0001)
- Largest positions deployed on Extreme Fear ($767 median), smallest on Extreme Greed ($500)
- L/S ratio flips from 2.21× net long (Extreme Fear) to 0.73× net short (Greed)

**Simulated upside:** If high-leverage traders had halved their position size on the losing side of Fear-day trades, total PnL on those trades would improve by an estimated **+$249,582** (actual $1,794,105 → simulated $2,043,687).

---

#### Rule 2 — Match strategy type to sentiment regime

*Applies to: all traders*

**Frequent / systematic traders:** Prioritise Fear-day setups. Win rate = 89.1% on Fear vs 74.5% on Greed — a 14.6pp gap. Running a systematic strategy during Greed conditions costs roughly 15 percentage points of win rate.

**Infrequent / momentum traders:** Prioritise Greed-day setups. Win rate = 91.8% on Greed vs 83.6% on Fear — an 8.2pp gap. Avoid forcing trades on Fear days where momentum conditions are absent.

**Evidence:** The win rate gap between regimes for each trader type is consistent throughout the full two-year dataset and not confined to any specific sub-period.

---

### Limitations

| Limitation | Detail |
|---|---|
| Sample selection | 32 accounts, all high-performing (WR 71%–100%); no struggling traders in sample |
| Bull market period | May 2023–May 2025 is predominantly bullish; Fear-day outperformance may be amplified by the cycle |
| Leverage proxy | Approximated from size / starting position, not directly observed margin settings |
| Causality | Correlation established between sentiment and outcomes; not causation |
| Trader count | Validation Check 6 flags 32 traders vs expected 16 from the original sample — full dataset has more accounts |

---

## Bonus: Predictive Model & Clustering (Notebook 10)

**Predictive model:** Random Forest classifier predicts next-day profitability bucket (Loss / Break-even / Profit) using today's sentiment score, rolling 3-day and 7-day win rate, rolling PnL, trade size, leverage, and long/short ratio. Rolling win rate and `fg_value` (raw sentiment score) rank as the top two features — confirming the sentiment signal carries genuine forward-looking information.

**Clustering:** KMeans (k=3) groups the 32 traders into behavioral archetypes based on leverage, frequency, trade size, win rate, and sentiment sensitivity. Each archetype maps to a distinct sentiment-performance profile consistent with the Rule 2 findings above.

---

*32 accounts · 211,068 trades · May 2023 – May 2025 · Mann-Whitney U significance testing throughout*

