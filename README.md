# Earnings Call NLP Analyser

> End-to-end NLP pipeline analysing earnings call language
> to measure management sentiment and test its relationship
> with post-earnings stock returns. FinBERT + VADER +
> SEC EDGAR + event study + trading signal backtest.

---

## What this project does

Earnings call transcripts contain language signals that
move stock prices. This project builds a production-ready
NLP pipeline that quantifies management sentiment using
both a general model (VADER) and a financial domain model
(FinBERT), then tests whether sentiment predicts
post-earnings abnormal returns using event study methodology.

---

## Companies analysed

AAPL · MSFT · TSLA · JPM · NVDA | 2020 Q1 to 2024 Q4
95 earnings call transcripts | 19 events per company

---

## Key findings

### NB3 — Model comparison
| Metric | Value |
|--------|-------|
| VADER vs FinBERT correlation | 0.031 |
| Interpretation | Near-zero models capture different signals |
| Max model divergence (TSLA) | 1.72 |
| Highest FinBERT score | AAPL 0.853 |
| Most volatile sentiment | TSLA std=0.749 |

Near-zero correlation confirms that financial domain
language requires domain-specific models. VADER
misclassifies financial hedging language that FinBERT
correctly identifies as negative.

### NB4 — Sentiment-return regression
| Model | Coefficient | p-value | R² | Significant |
|-------|------------|---------|-----|------------|
| VADER → CAR(0,+2) | +0.021 | 0.342 | 0.021 | No |
| FinBERT → CAR(0,+2) | -0.012 | 0.511 | 0.017 | No |
| Composite → CAR(0,+2) | -0.009 | 0.721 | 0.004 | No |
| Sentiment Change → CAR | -0.007 | 0.531 | 0.010 | No |

**Per-company (FinBERT → CAR):**
| Company | Coef | p-value | R² | Significant |
|---------|------|---------|-----|------------|
| AAPL | +0.014 | 0.145 | 0.026 | No |
| JPM | +0.034 | 0.121 | 0.121 | No |
| MSFT | -0.016 | 0.004 | 0.187 | Yes *** |
| NVDA | -0.090 | 0.357 | 0.048 | No |
| TSLA | -0.015 | 0.707 | 0.014 | No |

MSFT shows significant negative relationship —
consistent with management optimism bias documented
in Huang et al. (2014).

### NB5 — Trading signal backtest
| Signal | Trades | Excess vs BH | Sharpe | Win Rate |
|--------|--------|-------------|--------|----------|
| S1 Level (median) | 94 | +32.17% | 0.018 | 51.6% |
| S2 Change | 45 | -1.12% | -0.080 | 20.0% |
| S3 Composite | 94 | +32.17% | 0.018 | 51.6% |
| S4 High confidence | 86 | -19.33% | -0.153 | 33.7% |

Permutation test: strategy at 64th percentile
of 1000 random strategies (p=0.359).

**Per-company signal performance:**
| Company | Excess Return | Win Rate | Significant |
|---------|-------------|---------|------------|
| AAPL | +17.25% | 52.6% | No |
| JPM | +0.98% | 57.9% | No |
| MSFT | -13.70% | 31.6% | Yes ** |
| NVDA | +20.69% | 47.4% | No |
| TSLA | -3.96% | 36.8% | No |

---

## Methodology

### Text processing
- SEC EDGAR 8-K filing scraper with retry logic
- HTML cleaning and earnings section extraction
- Loughran-McDonald (2011) financial word lists
- Uncertainty ratio, forward-looking statement count
- Fog readability index

### Sentiment models
- **VADER** — general lexicon-based sentiment
- **FinBERT** (ProsusAI), financial domain BERT
  trained on 4.9B words of financial text
- Composite score: 60% FinBERT + 40% VADER
- Sentence-level and document-level scoring

### Event study
- Market model (CAPM) estimation window: 60 days
- Event window: CAR(0,+2), 3-day cumulative
- HC3 heteroskedasticity-robust standard errors
- Bonferroni correction for multiple testing
- 1000-permutation significance test

---

## Data note

Transcripts are synthetic, constructed from documented
earnings call language patterns with time-varying tone
probabilities reflecting real market periods
(NVDA AI boom 2023-2024, TSLA margin pressure 2022,
JPM rate uncertainty 2022).

The NLP pipeline is production-ready for real transcript
data from Refinitiv, Bloomberg, or Seeking Alpha Premium.
Connecting real data requires only replacing the data
loader — all downstream analysis is identical.

---

## Limitations

1. Synthetic transcripts, no real causal relationship
   to stock returns by construction
2. 19 events per company, severely underpowered
   (academic literature uses 1000+ events)
3. 3-day CAR window, market prices earnings in
   minutes, not days
4. No slippage, market impact, or short-selling
   constraints modelled

## Production pathway

1. Connect to Refinitiv/Bloomberg transcript API
2. Expand to 500+ quarterly events across sectors
3. Reduce event window to same-day or 1-day CAR
4. Add sector and market regime controls
5. Implement realistic execution constraints

---

## Notebooks

| Notebook | Description |
|----------|-------------|
| 01_edgar_scraping.ipynb | SEC EDGAR 8-K collection, synthetic dataset |
| 02_text_preprocessing.ipynb | VADER, linguistic features, QoQ change |
| 03_finbert_sentiment.ipynb | FinBERT, model comparison, divergence |
| 04_return_analysis.ipynb | Event study, CAR, sentiment regression |
| 05_trading_signals.ipynb | Signal backtest, permutation test |

---

## Tech stack

![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat)
![FinBERT](https://img.shields.io/badge/FinBERT-green?style=flat)
![VADER](https://img.shields.io/badge/VADER-orange?style=flat)
![HuggingFace](https://img.shields.io/badge/HuggingFace-yellow?style=flat)
![statsmodels](https://img.shields.io/badge/statsmodels-purple?style=flat)

---

## Related projects

- [Fama-French Factor Model](https://github.com/Kofi-An/fama-french-model)
  — Rolling OLS, TSLA alpha 116%, all portfolios beat SPY
- [Credit Risk Scorecard](https://github.com/Kofi-An/credit-risk-scorecard)
  — AUC 0.71, $275M loss reduction
- [Portfolio Risk Dashboard](https://kofi-an-portfolio-risk-dashboard.streamlit.app)
  — Live VaR, CVaR, Monte Carlo
- [Macro Dashboard](https://github.com/Kofi-An/macro-dashboard)
  — FRED + World Bank + Ghana

---

## Author

Kofi-An | Financial Data Scientist
Accra, Ghana | Open to remote roles globally

[GitHub](https://github.com/Kofi-An) ·
[LinkedIn](https://linkedin.com/in/your-handle) ·
[Portfolio](https://kofi-an.github.io)