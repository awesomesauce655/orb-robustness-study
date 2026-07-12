# Opening Range Breakout Robustness Study

## Overview

This project studies whether opening range breakout strategies are structurally robust or mostly dependent on narrow parameter choices.

The main question is:

**Are opening range breakout strategies robust across reasonable assumptions, or do they only look good under one specific setup?**

The project tests simple long-only, short-only, and long/short opening range breakout rules using QuantConnect minute data. The goal is not to force a profitable strategy, but to evaluate whether the ORB idea survives changes in opening window, asset, exit rules, filters, volatility regimes, and test period.

## Research Question

Are opening range breakout strategies structurally robust, or is their apparent profitability mostly driven by parameter selection?

## Initial Hypothesis

A genuinely robust ORB strategy should show similar economic behavior across neighboring assumptions.

If the strategy only performs well under one exact opening window, one exact exit rule, or one narrow filter, then the result is likely fragile or overfit.

## Strategy Concept

An opening range breakout strategy defines the high and low of the first X minutes after the market opens.

After the range is complete:

- A long breakout occurs when price breaks above the opening range high.
- A short breakout occurs when price breaks below the opening range low.

The baseline version is intentionally simple:

- Asset: QQQ
- Opening range: 5, 15, 30, or 60 minutes
- Direction: Long-only initially
- Entry: Break above opening range high
- Exit: End of day
- No stop-loss initially
- No profit target initially

## Tools Used

- QuantConnect
- Python
- Minute-level ETF data
- GitHub
- CSV experiment logging

## Assets Tested

The main asset tested was:

- QQQ — Nasdaq 100 ETF

SPY was also tested as a generalization asset.

IWM was intentionally skipped because the project focus became QQQ and SPY rather than small-cap ORB behavior.

## Key Experiments

The project tested:

- QQQ buy-and-hold benchmark
- SPY buy-and-hold benchmark
- QQQ ORB with 5-, 15-, 30-, and 60-minute opening windows
- SPY 5-minute ORB
- Opening-range-low stop
- 2R profit target
- Noon exit
- VWAP confirmation
- 200-day moving average filter
- Gap-up and gap-down filters
- Short-only ORB
- Long/short ORB
- High-volatility and low/medium-volatility regime filters
- 20-day momentum filter
- 2022–2026 subperiod robustness check

## Experiment Summary

| Experiment | Strategy | Asset | CAGR | Sharpe | Max Drawdown | Notes |
|---|---|---:|---:|---:|---:|---|
| EXP-001 | 15m Long ORB | QQQ | 4.880% | 0.069 | 20.100% | Positive but weak baseline |
| EXP-002 | 5m Long ORB | QQQ | 7.342% | 0.203 | 16.100% | Best simple ORB version |
| EXP-003 | 30m Long ORB | QQQ | 4.584% | 0.045 | 18.400% | Weaker than 5m |
| EXP-004 | 60m Long ORB | QQQ | 4.889% | 0.057 | 16.700% | Weak but positive |
| EXP-005 | Buy-and-Hold | QQQ | 18.987% | 0.577 | 36.600% | Strongly beat ORB on return and Sharpe |
| EXP-006 | 5m Long ORB | SPY | 4.892% | 0.063 | 13.200% | Did not generalize strongly |
| EXP-007 | Buy-and-Hold | SPY | 14.290% | 0.471 | 33.600% | Beat SPY ORB on return and Sharpe |
| EXP-008 | 5m ORB + OR Low Stop | QQQ | 6.206% | 0.152 | 10.800% | Better drawdown, lower return |
| EXP-009 | 5m ORB + Stop + 2R Target | QQQ | 0.628% | -0.338 | 12.300% | Profit target hurt badly |
| EXP-010 | 5m ORB + Noon Exit | QQQ | 4.873% | 0.057 | 13.300% | Early exit reduced upside |
| EXP-011 | 5m ORB + VWAP Filter | QQQ | 7.342% | 0.203 | 16.100% | VWAP filter was redundant |
| EXP-012 | 5m ORB + 200DMA Filter | QQQ | 1.306% | -0.206 | 11.800% | Trend filter hurt performance |
| EXP-013 | 5m ORB + Gap-Up Filter | QQQ | 5.593% | 0.110 | 13.300% | Reduced risk but also return |
| EXP-014 | 5m ORB + Gap-Down Filter | QQQ | 1.099% | -0.230 | 27.800% | Performed poorly |
| EXP-015 | 5m Short-Only ORB | QQQ | -5.521% | -0.481 | 39.100% | Short side was weak |
| EXP-016 | 5m Long/Short ORB | QQQ | 0.656% | -0.108 | 47.700% | Short side damaged results |
| EXP-017 | 5m ORB + High-Vol Filter | QQQ | 5.339% | 0.091 | 15.600% | High vol helped more than low/medium, but not enough |
| EXP-018 | 5m ORB + Low/Med-Vol Filter | QQQ | 1.905% | -0.180 | 10.600% | Too restrictive |
| EXP-019 | 5m ORB + 20D Momentum Filter | QQQ | 2.590% | -0.140 | 21.400% | Momentum filter hurt |
| EXP-020 | 5m ORB, 2022–2026 | QQQ | 8.439% | 0.159 | 15.400% | Subperiod remained positive but weak |

Full results are available in:

results/experiment_log.csv