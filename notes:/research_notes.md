# Research Notes — Opening Range Breakout Robustness Study

## Current Stage

This project is currently testing simple long-only opening range breakout behavior before adding more complex filters.

The goal is not to optimize the strategy yet. The goal is to understand whether the basic ORB idea behaves consistently across reasonable assumptions.

## Baseline Setup

The baseline strategy uses:

- Asset: QQQ
- Direction: Long-only
- Data: Minute resolution
- Entry: Break above the opening range high
- Exit: End of day
- Stop-loss: None initially
- Profit target: None initially
- Backtest period: 2020-01-01 to 2026-07-01

## Opening Window Tests

The first robustness tests compared different opening range windows:

- 5-minute ORB
- 15-minute ORB
- 30-minute ORB
- 60-minute ORB

The 5-minute version performed best among the tested ORB windows. It produced the highest CAGR, Sharpe, Sortino, and net profit among the simple QQQ ORB variants.

However, the other opening windows were much weaker. The 15-minute, 30-minute, and 60-minute versions were all positive, but their Sharpe ratios were very low.

This suggests that opening range window choice matters a lot. The strategy does not yet look broadly robust across opening windows.

## Benchmark Comparison

QQQ buy-and-hold strongly outperformed all QQQ ORB variants on raw return and Sharpe.

The best ORB version so far, QQQ 5-minute ORB, had lower drawdown than QQQ buy-and-hold, but much lower return and lower Sharpe.

This means the simple ORB strategy should not be described as outperforming buy-and-hold. Its only clear advantage so far is reduced drawdown due to lower market exposure.

## Asset Comparison

The QQQ 5-minute ORB result did not generalize strongly to SPY.

SPY 5-minute ORB had lower drawdown, but it was much weaker than QQQ 5-minute ORB on CAGR, Sharpe, Sortino, and net profit.

This suggests the strongest ORB result so far may be asset-dependent.

## Stop-Loss Test

Adding an opening-range-low stop to the QQQ 5-minute ORB reduced average loss and drawdown significantly.

However, the stop also reduced CAGR and net profit.

This suggests the stop improved risk control, but may have cut off trades that later recovered during the day.

## Profit Target Test

Adding a 2R profit target to the QQQ 5-minute ORB with an opening-range-low stop performed poorly.

The 2R target severely reduced performance and produced negative Sharpe and Sortino ratios.

This suggests that capping winners at 2R may remove too much upside from the best ORB trades.

## Time Exit Test

Changing the exit from end of day to noon reduced drawdown, but also materially reduced return and Sharpe.

This suggests that many of the better ORB trades may need more time to develop beyond the morning session.

## Intermediate Finding

The best simple version so far is:

- QQQ
- 5-minute opening range
- Long-only breakout
- No stop
- No profit target
- End-of-day exit

However, even this version does not outperform QQQ buy-and-hold on raw return or Sharpe.

The honest current conclusion is that simple ORB rules may reduce drawdown by limiting market exposure, but the tested versions do not yet show strong evidence of a robust standalone edge.

## What This Means For Next Tests

The next tests should not randomly add filters just to improve results.

Instead, each new rule should answer a specific research question, such as:

- Does trend confirmation improve breakout quality?
- Does a gap filter separate stronger from weaker ORB days?
- Does volatility regime affect ORB performance?
- Does VWAP confirmation reduce false breakouts?
- Does short-side ORB behavior differ from long-side ORB behavior?

## Current Warning

The project already shows signs of parameter sensitivity.

The 5-minute window looks much better than the other opening windows, while stop-loss, profit target, and noon exit variants weakened performance.

That means we should be careful not to overstate the result or cherry-pick the best version.