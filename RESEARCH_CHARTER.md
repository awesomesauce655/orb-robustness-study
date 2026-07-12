# Research Charter — Opening Range Breakout Robustness Study

## Project Title

Opening Range Breakout Robustness Study

## Research Question

Are opening range breakout strategies structurally robust, or is their apparent profitability mostly driven by parameter selection?

## Plain-English Version

If we test different opening range windows, stop-loss rules, profit targets, and exit times, does the strategy behave consistently, or does it only look good under one cherry-picked setup?

## Initial Hypothesis

A genuinely robust opening range breakout strategy should show similar economic behavior across neighboring parameter choices.

If the strategy only performs well with one exact opening range window, one exact stop, or one exact exit rule, then the result is likely fragile or overfit.

## Strategy Concept

An opening range breakout strategy defines the high and low of the first X minutes after the market opens.

After the opening range is complete, the strategy waits for price to break above the opening range high or below the opening range low.

A simple long-only version enters when price breaks above the opening range high and exits by the end of the trading day.

## Baseline Strategy

The baseline strategy will use:

- Asset: QQQ
- Opening range window: 15 minutes
- Direction: Long-only
- Entry: Buy when price breaks above the opening range high after the range is complete
- Exit: End of day
- Stop-loss: None initially
- Profit target: None initially
- Position sizing: 100% allocation
- Data resolution: Minute data
- Trading session: Regular market hours only

## Why Start Simple?

The baseline should be intentionally simple so the first result is easy to interpret.

Adding filters too early, such as VWAP, volume, trend, volatility, or gap filters, can make the strategy look better while increasing the risk of overfitting.

## Variables to Test Later

After the baseline is built, robustness tests may include:

- Opening range windows: 5 minutes, 15 minutes, 30 minutes, 60 minutes
- Assets: QQQ, SPY, IWM
- Direction: Long-only, short-only, long/short
- Exit rules: End of day, time-based exit, opposite breakout
- Stop-loss rules: Opening range low, fixed percent stop, ATR-based stop
- Profit targets: Fixed reward/risk targets, trailing exits
- Filters: VWAP, volume, gap size, trend, volatility regime
- Time-of-day restrictions
- Transaction cost and slippage assumptions

## What Would Support the Hypothesis?

The hypothesis would be supported if the ORB strategy shows similar behavior across reasonable neighboring parameter choices.

For example, if 5-minute, 15-minute, and 30-minute opening ranges all show similar directional behavior, similar risk-adjusted performance, and similar drawdown patterns, the strategy may be structurally meaningful.

## What Would Weaken or Reject the Hypothesis?

The hypothesis would be weakened if performance depends on one narrow parameter combination.

For example, if only the 15-minute window works while 5-minute, 30-minute, and 60-minute versions perform poorly, then the result may be overfit.

The hypothesis would also be weakened if the strategy performs well before costs but breaks after transaction costs, slippage, or more realistic execution assumptions.

## Key Risks

- Look-ahead bias
- Overfitting parameters
- Survivorship bias if asset selection is too narrow
- Ignoring transaction costs
- Ignoring slippage
- Unrealistic fills on breakout bars
- Time-zone mistakes
- Accidentally using premarket data
- Confusing the opening range formation period with the trading period
- Optimizing too many filters too early

## First Exit Test

Before coding, I should be able to explain:

1. How the opening range high and low are formed.
2. Why a 15-minute ORB cannot trade before 9:45 AM ET.
3. Why testing multiple opening windows matters.
4. Why adding too many filters too early increases overfitting risk.

## Research Standard

The goal is not to force the ORB strategy to look profitable.

The goal is to test whether the idea survives realistic, neighboring assumptions.

A failed or fragile ORB result is still useful if the process is honest and well-documented.