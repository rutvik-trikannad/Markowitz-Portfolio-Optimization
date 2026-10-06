# Markowitz Portfolio Optimization

A Python pipeline that builds optimal portfolios from 14 stocks across 7 sectors using Markowitz mean-variance optimization, then backtests them against SPY, QQQ, and XLK. It runs from raw price data to a finished Excel file and PDF report, and uses six years of daily prices from Yahoo Finance (August 2020 to August 2026).

The Maximum Sharpe portfolio reached a Sharpe ratio of 1.66 against 0.71 for SPY over the same window. The Minimum Variance portfolio had a maximum drawdown of -13.75% against -24.50% for SPY. Both results are in-sample, so read them as a best-case view and not a forecast (see Limits).

## Key results

| | Min Variance | Max Sharpe | SPY | QQQ | XLK |
|---|---|---|---|---|---|
| Sharpe ratio | 1.06 | 1.66 | 0.71 | 0.65 | 0.77 |
| Annualised volatility | 13.67% | 18.84% | 16.83% | 22.73% | 25.25% |
| Maximum drawdown | -13.75% | -19.80% | -24.50% | -35.12% | -33.56% |
| Cumulative return | 196% | 667% | 149% | 175% | 247% |
| Alpha vs SPY | 7.77% | 24.60% | | -1.22% | 2.29% |

Both optimal portfolios are capped at 15% per stock. The Min Variance portfolio holds PG and COST at the cap and spreads the rest across defensive names in energy, financials, and healthcare. The Max Sharpe portfolio puts five stocks at the cap (NVDA, LLY, COST, XOM, CAT), which shows the unconstrained optimizer would concentrate even more.

## How it works

1. **Data.** Six years of adjusted daily prices for 14 stocks in 7 sectors (Technology, Semiconductors, Financials, Healthcare, Consumer Staples, Energy, Industrials), plus the live 10-year Treasury yield as the risk-free rate (4.682% on August 12, 2026).
2. **Opportunity set.** 100,000 randomly weighted portfolios, plotted on risk and return to show the shape of the efficient frontier.
3. **Optimization.** SciPy's SLSQP solver finds the exact Min Variance and Max Sharpe portfolios. The only constraints are that weights sum to 100% and each stock is between 0% and 15%. There is no sector constraint.
4. **Efficient frontier.** The exact frontier is traced by minimizing variance at 60 target return levels and overlaid on the simulated portfolios.
5. **Backtest.** Both portfolios against SPY, QQQ, and XLK on Sharpe, Sortino, maximum drawdown, alpha (single-factor CAPM against SPY), and active share (against an equal-weighted version of the 14 stocks).
6. **Risk and rebalancing.** Daily 95% and 99% VaR and CVaR, a rolling 252-day Sharpe ratio, a drawdown chart, and a monthly rebalancing simulation compared with buy and hold.
7. **Reports.** The notebook writes the weights to Excel with openpyxl and builds the PDF report with ReportLab.

## Limits

- **In-sample.** The weights are fitted to the same six years that the backtest then measures, and expected returns come from historical averages. Mean-variance weights are very sensitive to those inputs, so the outperformance would likely shrink out of sample.
- **No trading costs.** The backtest does not include transaction costs or taxes.
- **Long-only equities.** There are no sector limits, derivatives, or bonds in the universe.
- **Moving window.** The notebook pulls the latest six years of data, so rerunning it on a later date gives a different window and slightly different results. The numbers here are from the August 12, 2026 run, and the random simulation uses a fixed seed of 42.

## Files

- `Markowitz_Portfolio_Optimization.ipynb`: the full notebook
- `Portfolio_Optimization_Report.pdf`: an auto-generated summary of the method, results, and charts. It is a structured overview, and the notebook is the primary source for the full analysis
- `weights.xlsx`: formatted export of the optimal weights, with positions at the 15% cap flagged
- `efficient_frontier_curve.png`: the efficient frontier with the exact optimization curve
- `correlation_heatmap.png`: pairwise correlations across the 14 stocks
- `rolling_sharpe.png`: 252-day rolling Sharpe ratio against the benchmarks
- `drawdown_chart.png`: drawdown from peak against the benchmarks
- `rebalancing_comparison.png`: monthly rebalanced against buy and hold
- `var_cvar.png`: daily VaR and CVaR at 95% and 99%
- `requirements.txt`: Python dependencies

## Running it

Run the notebook in Jupyter or Google Colab with an internet connection, since prices come from Yahoo Finance. Install the dependencies first with `pip install -r requirements.txt`.

This is a student project and not investment advice.

