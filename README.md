# Bitcoin Stylized Facts & Tail Risk  
**Historical VaR/CVaR, VaR Backtesting (Kupiec), and EVT (POT–GPD) on BTC Daily Data**

## Overview
This repository provides an end-to-end empirical analysis of **Bitcoin (BTC) daily returns** with two goals:

1. Document key **stylized facts** (non-normality, heavy tails, volatility clustering, drawdowns)  
2. Quantify and validate **tail risk** using:
   - Historical **VaR / CVaR** (including rolling estimates)
   - **Kupiec POF** VaR backtesting
   - **EVT (POT–GPD)** for extreme crash quantiles

> Results in this repo were produced on Binance BTCUSDT daily data from Kaggle (2018-01-01 → 2025-12-08).

---

## Data
- **Asset:** BTCUSDT  
- **Frequency:** Daily bars  
- **Source:** Binance BTCUSDT daily dataset from Kaggle  
- **Coverage:** **2018-01-01 → 2025-12-08**  
- **Raw size:** **2899 rows × 11 columns**  
- **Quality checks:** Missing bars = 0, negative prices = False, High < Low rows = 0

Place your cleaned daily CSV in either:
- `data/processed/btc_1d_features.csv` (recommended), or
- adjust paths in `scripts/run_pipeline.py`.

Raw data should remain local (not committed) under `data/raw/`.

---

## Methods
- **Returns:** log returns \(r_t = \ln(C_t) - \ln(C_{t-1})\)  
- **Stylized facts:** summary stats, JB & D’Agostino normality tests, ACF, ARCH LM  
- **Tail risk:** historical VaR/CVaR, rolling VaR/CVaR (250-day), exceptions plot  
- **Backtesting:** Kupiec POF (unconditional coverage)  
- **EVT:** POT threshold on losses + GPD fit + extreme VaR extrapolation

---

## Key Results (from sample run)
Returns diagnostics (n=2809):
- mean: 0.000916  
- std: 0.033662  
- skew: -1.183  
- excess kurtosis: 20.437  

Normality:
- Jarque–Bera: 49357.84 (p≈0)
- D’Agostino K²: 1132.72 (p≈1.08e-246)

Volatility clustering:
- ARCH LM (12 lags): LM=47.04 (p≈4.58e-06)

Tail risk:
- Historical 5% VaR=-0.0518, CVaR=-0.0797  
- Historical 1% VaR=-0.0937, CVaR=-0.1321  

VaR backtest (rolling 1% VaR, 250d):
- T=2559, X=32, p̂=0.01250
- Kupiec LR=1.5024, p-value=0.2203

EVT (POT–GPD, tail_prob=0.05):
- threshold u=0.05181, exceedances=141
- xi=0.2052, beta=0.02174
- EVT VaR 1%: 0.0934 loss
- EVT VaR 0.5%: 0.1159 loss
- EVT VaR 0.1%: 0.1825 loss

---

## Repository Layout
- `src/` reusable functions (features, stylized facts, tail risk, backtesting, EVT)
- `scripts/run_pipeline.py` one-command pipeline that writes `reports/results.json`
- `notebooks/` step-by-step notebooks
- `reports/figures/` exported figures (placeholders included)

---

## Quickstart
```bash
pip install -r requirements.txt
python scripts/run_pipeline.py
```

Outputs:
- `reports/results.json` (key numbers)
- You can export plots to `reports/figures/`

---

## Disclaimer
For research/education only. Not investment advice.
