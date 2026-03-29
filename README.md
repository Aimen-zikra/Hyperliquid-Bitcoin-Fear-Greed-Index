# Hyperliquid-Bitcoin-Fear-Greed-Index
Trader Behavior &amp; Performance Analysis - How Market Sentiment Drives Trader Behavior on Hyperliquid

Task: Analyze whether Bitcoin market sentiment (Fear/Greed Index) explains differences in Hyperliquid trader behavior and performance. Produce actionable strategy rules backed by data.

----

Repository Structure
│
├── notebooks/
│   ├── 01_data_understanding.ipynb   # Dataset audit, viability check
│   ├── 02_data_extraction.ipynb      # Load, parse, merge, engineer metrics
│   ├── 03_data_cleaning.ipynb        # Remove noise, standardise, export
│   ├── 04_eda.ipynb                  # Distributions, trends, per-trader profiles
│   ├── 05_deep_analysis.ipynb        # Answer the 3 core analysis questions
│   ├── 06_validation.ipynb           # Cross-checks, sanity tests
│   ├── 07_insights.ipynb             # Findings formatted as insight cards
│   ├── 08_recommendations.ipynb      # Strategy rules with evidence
│   ├── 09_visualization.ipynb        # Presentation-ready charts
│   
│
├── charts/
│   ├── CHART_01_dashboard.png        # 3-metric sentiment dashboard
│   ├── CHART_02_segment_sensitivity.png
│   ├── CHART_03_before_after.png
│   └── CHART_04_cumulative_pnl.png
│
└── README.md

Setup
Requirements
bashpip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
Python version
Python 3.9+ recommended.
Data files
Place both source CSVs in the data/ folder before running:

fear_greed_index.csv — Bitcoin Fear/Greed Index
historical_data.csv — Hyperliquid trade history

How to run
Run notebooks in order — each notebook reads the output of the previous one:
bashjupyter notebook
Open and run cells top-to-bottom in sequence: 01 → 02 → ... → 10.
Each notebook reads from and writes to the data/ folder. The key file chain is:
historical_data.csv + fear_greed_index.csv
        ↓ (02_data_extraction)
  working_trades.csv
        ↓ (03_data_cleaning)
  clean_trades.csv  +  daily_per_trader.csv
        ↓ (04 through 10)
  charts, insights, model outputs

Datasets
Dataset	Rows	Columns	Period
Bitcoin Fear/Greed Index	2,644	4	Feb 2018 – May 2025
Hyperliquid Trade Records	211,224 (raw) → 211,068 (clean)	16 → 22	May 2023 – May 2025
