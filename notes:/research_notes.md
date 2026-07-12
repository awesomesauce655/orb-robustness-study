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

## Confirmation Filter Tests

After testing opening windows, stops, profit targets, and time exits, the next set of experiments tested whether confirmation filters improved QQQ 5-minute ORB performance.

The goal was not to randomly add filters, but to ask whether certain market conditions improved breakout quality.

## VWAP Confirmation

The VWAP confirmation test required price to break above the 5-minute opening range high and also be above intraday VWAP.

This produced identical results to the no-filter 5-minute ORB.

This suggests the VWAP filter was redundant in this setup. If QQQ broke above the 5-minute opening range high, it was already effectively above VWAP most of the time.

VWAP confirmation did not improve the strategy or meaningfully change trade selection.

## 200-Day Moving Average Filter

The 200-day moving average filter required QQQ to be above its 200-day moving average before taking long ORB breakouts.

This filter reduced trades and drawdown, but it severely weakened return and risk-adjusted performance.

This suggests that the long ORB strategy did not simply work better during broad bullish trend periods. The filter may have skipped too many profitable rebound or transitional trades.

## Gap-Up Filter

The gap-up filter only allowed long ORB breakouts on days when QQQ opened above the previous close.

This reduced trades, fees, turnover, and drawdown compared with the no-filter version.

However, it also reduced CAGR, Sharpe, Sortino, and net profit. The filter improved selectivity, but not enough to beat the simple no-filter 5-minute ORB.

## Gap-Down Filter

The gap-down filter only allowed long ORB breakouts on days when QQQ opened below the previous close.

This performed poorly. Return was low, Sharpe and Sortino were negative, and drawdown was much higher than the gap-up version.

This suggests that long ORB breakouts after weak opens were much riskier and did not provide strong evidence of a rebound edge.

## Updated Intermediate Finding

The simple QQQ 5-minute ORB remains the best version tested so far.

Most filters reduced exposure, but none improved the strategy versus the no-filter version.

The current pattern is:

- VWAP confirmation was redundant.
- 200-day moving average confirmation was too restrictive.
- Gap-up confirmation reduced risk but also reduced return.
- Gap-down confirmation performed poorly.
- Stops and early exits improved risk control but reduced upside.
- A 2R profit target hurt performance badly.

The most honest conclusion so far is that QQQ 5-minute ORB has some positive behavior, but the edge is fragile. The strategy is sensitive to opening window choice, and most added filters have not improved it.

This project is increasingly showing that simple ORB performance may be more parameter-sensitive than structurally robust.

## Directional Tests

The next set of experiments tested whether QQQ opening range breakout behavior was stronger on the long side, short side, or both.

## Short-Only ORB

The short-only version entered when price broke below the 5-minute opening range low and exited at the end of the day.

This performed poorly. The strategy had negative CAGR, negative Sharpe, negative Sortino, and a large drawdown.

This suggests that short-side QQQ ORB breakouts were not structurally strong over the test period. The strategy likely fought the broader upward drift in QQQ.

## Long/Short ORB

The long/short version entered long on a break above the opening range high or short on a break below the opening range low, with only one trade per day.

This also performed poorly. Adding the short side did not diversify the strategy. Instead, it diluted the positive long-side behavior and added significant drawdown.

## Directional Finding

QQQ ORB behavior appears asymmetric:

- Long-only ORB showed weak but positive behavior.
- Short-only ORB performed poorly.
- Long/short ORB performed much worse than long-only.

The current evidence suggests that QQQ ORB should be studied primarily as a long-side strategy, not as a symmetric long/short breakout system.

## Volatility Regime Tests

The next experiments tested whether QQQ 5-minute ORB performance depended on recent volatility conditions.

This connected Project 2 back to Project 1, where volatility regimes were important for momentum behavior.

## High-Volatility Filter

The high-volatility filter only allowed long ORB trades when 20-day realized volatility was above the 66.7th percentile of recent volatility history.

This reduced trades, fees, and turnover significantly.

However, it did not improve the strategy versus the no-filter 5-minute ORB. CAGR, Sharpe, Sortino, and net profit were all lower than the simple no-filter version.

High-volatility days produced larger average wins, but also larger average losses. This suggests that volatility created more movement, but not necessarily better risk-adjusted breakout quality.

## Low/Medium-Volatility Filter

The low/medium-volatility filter did the opposite. It only allowed long ORB trades when 20-day realized volatility was not high.

This produced lower drawdown, but return and risk-adjusted performance were much weaker.

Low/medium-volatility ORB had negative Sharpe and Sortino, suggesting that avoiding high-volatility days removed too many of the meaningful breakout opportunities.

## Volatility Finding

For QQQ 5-minute ORB, high-volatility days were better than low/medium-volatility days.

However, neither volatility filter beat the no-filter 5-minute ORB.

This suggests that the ORB strategy may benefit from some volatility, but filtering trades strictly by volatility regime was too restrictive.

## Connection to Project 1

This result differs from Project 1.

In Project 1, QQQ momentum performed better when avoiding high-volatility regimes.

In Project 2, QQQ ORB performed better in high-volatility regimes than in low/medium-volatility regimes, although the no-filter version still performed best overall.

This is an important research insight: different strategy types can respond differently to volatility regimes.

Momentum and intraday breakout strategies should not automatically use the same regime assumptions.

## Short-Term Momentum Filter

The 20-day momentum filter tested whether QQQ 5-minute ORB worked better after recent upward price strength.

The filter only allowed long ORB trades when QQQ had positive 20-day momentum.

This reduced the number of trades, but it did not improve performance. CAGR, Sharpe, Sortino, and net profit all fell versus the no-filter 5-minute ORB. Drawdown also increased.

This suggests that QQQ ORB does not simply work better after recent 20-day strength.

## Updated Research Direction

The evidence is becoming clearer:

- The simple QQQ 5-minute long-only ORB remains the best tested version.
- Most filters reduce exposure but do not improve the strategy.
- Short-side and long/short ORB variants performed poorly.
- Volatility, gap, trend, VWAP, and momentum filters did not beat the simple no-filter version.
- QQQ buy-and-hold still strongly outperforms ORB on raw return and Sharpe.

This suggests the ORB idea may be parameter-sensitive rather than structurally robust.

At this stage, the project does not need endless additional filters. The next step should be to organize the results, write a clear conclusion, and decide whether one or two final robustness checks are worth running before packaging.

## Subperiod Robustness Check

The final robustness check tested the best simple strategy over a shorter and more recent period.

The tested version was:

- QQQ
- 5-minute opening range
- Long-only breakout
- No stop
- No profit target
- End-of-day exit
- Test period: 2022-01-01 to 2026-07-01

This subperiod test remained positive. The strategy did not collapse when the 2020–2021 period was removed.

However, the Sharpe ratio remained low, which means the result still should not be overstated.

## Subperiod Finding

The subperiod test gives the QQQ 5-minute ORB slightly more credibility because the strategy was not only dependent on the full 2020–2026 test period.

However, the result still supports the broader conclusion:

- QQQ 5-minute ORB has positive behavior.
- The edge appears weak.
- The strategy remains much weaker than QQQ buy-and-hold on raw return and Sharpe.
- Most filters and modifications failed to improve it.
- The strategy is sensitive to parameter and rule choices.