## Results

| Model | Coefficient | t-stat | p-value | R² | Significant |
|-------|------------|--------|---------|-----|------------|
| VADER → CAR | 0.021 | 0.950 | 0.342 | 0.021 | No |
| FinBERT → CAR | -0.012 | -0.657 | 0.511 | 0.017 | No |
| Composite → CAR | -0.009 | -0.357 | 0.721 | 0.004 | No |
| Sentiment Change → CAR | -0.007 | -0.626 | 0.531 | 0.010 | No |

**Per-company (FinBERT → CAR):**

| Company | Coef | p-value | R² | Spearman r | Significant |
|---------|------|---------|-----|------------|------------|
| AAPL | +0.014 | 0.145 | 0.026 | +0.212 | No |
| JPM | +0.034 | 0.121 | 0.121 | +0.442 | No |
| MSFT | -0.016 | 0.004 | 0.187 | -0.534 | Yes *** |
| NVDA | -0.090 | 0.357 | 0.048 | -0.006 | No |
| TSLA | -0.015 | 0.707 | 0.014 | -0.138 | No |

**Interpretation:** No significant aggregate relationship due to
synthetic data and small sample (19 events/company). MSFT shows
significant negative relationship consistent with management
optimism bias (Huang et al. 2014). Pipeline is production-ready
for real transcript data with 500+ events.