# From Mean-Variance Optimization to Factor-Neutral Portfolios (KO and PEP)

How sensitive are the "optimal" weights of a two-stock portfolio (Coca-Cola and PepsiCo) to small errors in expected returns, and can regularisation and factor neutrality make them more stable?

Group project for the Master of Science in Quantitative Finance at Singapore Management University (SMU).

## Key results

Data: daily returns for KO and PEP, 2021-01-05 to 2025-12-31 (1,254 days). Risk-aversion setting: 4.

| | Naive optimiser | Ridge (strength 0.0001) | Factor-neutral |
|---|---|---|---|
| KO weight | +111.1% | +69.8% | +27.6% |
| PEP weight | -57.5% | -27.0% | -26.2% |
| Total size of positions (gross) | 168.6% | 96.8% | 53.8% |
| Annual volatility (risk) | 12.9% | 8.5% | 3.6% |

- **The naive optimiser is extremely sensitive.** Raising KO's expected daily return by just 1 basis point (0.01%, about 2.5% a year) moves the KO weight by +47.8 percentage points and the PEP weight by -29.2. A 5 bp change moves them by +239 and -146 points. For comparison, KO's estimation error is about 2.8 bp a day.
- **Ridge shrinks the weights and cuts turnover.** The same shocks cause about 42% less trading (KO shocks 42.4%, PEP shocks 41.4%).
- **The factor-neutral portfolio has zero exposure to the market** by construction, and the lowest risk of the three.

Plain-language terms:
- **Mean-variance optimisation (MVO):** choosing portfolio weights that balance expected return against risk.
- **Ridge stabilisation:** adding a penalty that pulls extreme weights toward zero.
- **Turnover:** how much you have to trade when the weights change.
- **Factor-neutral:** a portfolio built to have no exposure to a chosen factor (here, the overall stock market).
- **Alpha / beta:** return not explained by the market (alpha) versus return that moves with it (beta).
- **Gross exposure:** the sum of the absolute sizes of all positions, long and short.

## What the project does

1. **Estimate the inputs.** Average daily returns, the covariance matrix (how the two stocks move together) and the correlation (0.69) from the data.
2. **Build a one-factor risk model.** Regress each stock's excess return (return minus the risk-free rate) on the market's excess return. The market explains only about 9% of each stock's moves.
3. **Naive MVO.** Solve for the weights, then nudge the expected returns by 1, 2, 3 and 5 bp and record how far the weights move.
4. **Ridge MVO.** Repeat with a ridge penalty at five strengths (0, 0.0001, 0.0005, 0.001, 0.005). Use 0.0001 for the shock tests and compare turnover with the naive version.
5. **Factor-neutral portfolio.** Start from the regression alphas, then remove all market exposure. Compare the risk of all three portfolios.

## Repository contents

| File | Description |
|---|---|
| `Minimum_Variance_Portfolio_fixed.ipynb` | The full analysis |
| `prices_KO_PEP.csv` | Daily adjusted closing prices for KO and PEP |
| `ff_factors_daily.csv` | Fama-French daily market factor and risk-free rate (in percent) |
| `download_data.py` | Optional: re-downloads the two CSV files |
| `requirements.txt` | Python packages needed |

## How to run

1. Install Python 3, then run `pip install -r requirements.txt`.
2. Keep the notebook and both CSV files in the same folder.
3. Open the notebook with `jupyter notebook` and choose **Kernel > Restart & Run All**.

To refresh the data, run `python download_data.py` (needs `yfinance` and `pandas-datareader`).

## Data

- Prices: Yahoo Finance via the `yfinance` package, adjusted for dividends and splits, 2021-01-04 to 2025-12-31.
- Market factor and risk-free rate: Kenneth R. French Data Library, "Fama/French 3 Factors [Daily]", 2021-01-04 to 2025-12-31.

## Limitations

- Only two stocks, so the results illustrate the method rather than a realistic portfolio.
- Everything is estimated and measured on the same five years of data, with no out-of-sample test.
- The ridge strength was chosen, not tuned, and part of the lower turnover comes from smaller positions. Per unit of position size, the naive and ridge portfolios change by about the same amount (0.46 each).
- The factor-neutral weights come from alphas that are not statistically different from zero (t-statistics 0.53 and -0.25), so they are an example of the method, not a trading signal.
- The one-factor model treats KO and PEP as linked only through the market (implied correlation 0.09, versus 0.69 in the data). Risk in the table is therefore measured with the sample covariance matrix.
- No transaction costs, position limits or leverage limits.

## Authors

- Eric Jansen
- Davidson Joel Mundt
- Diyaa Rajeev Valiyaparambath Kuttikattil
