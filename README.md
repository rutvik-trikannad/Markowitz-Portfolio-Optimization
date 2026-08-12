# Markowitz Portfolio Optimization

A mean-variance portfolio optimization framework built in Python across 14 stocks spanning 7 sectors (Technology, Semiconductors, Financials, Healthcare, Consumer Staples, Energy, Industrials).

Runs 100,000 Monte Carlo simulated portfolios to map the efficient frontier, then uses scipy's SLSQP solver to find the exact Min Variance and Max Sharpe portfolios (with a 15% position cap per stock). Backtests both against SPY, QQQ, and XLK over a 6-year window, with a full performance suite: Sharpe, Sortino, max drawdown, alpha, active share, VaR/CVaR, rolling Sharpe, and a monthly rebalancing simulation.

## Files
- `Markowitz_Portfolio_Optimization.ipynb` — the notebook
- `Portfolio_Optimization_Report.pdf` — a programmatically generated summary: methodology, optimization results, key metrics, and all charts in one place. It's a structured overview rather than a fully narrated research report; the notebook is the primary source for the full analysis
- `weights.xlsx` — formatted export of the optimal portfolio weights
- `correlation_heatmap.png` — pairwise correlation matrix across the 14-stock universe
- `efficient_frontier_curve.png` — the efficient frontier with the exact optimization curve
- `rolling_sharpe.png` — 252-day rolling Sharpe ratio vs benchmarks
- `drawdown_chart.png` — drawdown from peak vs benchmarks
- `rebalancing_comparison.png` — monthly rebalanced vs buy-and-hold
- `var_cvar.png` — daily VaR and CVaR at 95%/99% confidence
- `requirements.txt` — Python dependencies

## Requirements

Run in Jupyter or Google Colab with an active internet connection (for live data via yfinance). Install dependencies with: `pip install -r requirements.txt`

