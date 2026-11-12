# Solana Market-Cap Forecasting Entry

## Overview

This entry forecasts the five new Solana tokens with the highest market capitalization 24 hours after the snapshot window closes. It is a deterministic Python 3 program that uses only the standard library. The solution reads historical 10-minute snapshots from the house's input directory and produces a single forecast file containing per-place coin probabilities, an ordered top-5 guess, and per-place market-cap size probabilities.

## Method

The core of the approach is a Monte Carlo simulation of each candidate coin's market-cap path over the relevant forecast horizons. By simulating these paths, we estimate the probability that each specific coin occupies each rank (1st through 5th) at the target time.

To address the challenge of unnamed coins (those not present in the historical data), we employ a zero-probability naming trick to reduce the share of probability mass assigned to unknown entities. 

The size model for market capitalization is constructed as a horizon mixture of narrow per-coin distributions. Additionally, we define separate newcomer size distributions for each place:
- **First Place:** Highest center value and tightest spread.
- **Later Places:** Progressively lower centers and wider spreads.

Crucially, the simulation draws several brand-new coins for each path. This reflects the empirical observation that most top-5 winners are coins that were not seen in the historical data prior to the snapshot window.

## Inputs

The program expects to find 10-minute interval snapshots of Solana token market caps in the designated input directory. These files serve as the historical basis for parameterizing the simulation and identifying existing candidates.

## Outputs

The program writes a single forecast file. This file includes:
1. **Per-place coin probabilities:** The likelihood of each identified or simulated coin occupying ranks 1–5.
2. **Ordered top-5 guess:** A deterministic list of the five predicted highest market-cap tokens.
3. **Per-place market-cap size probabilities:** Distribution estimates for the market cap values associated with each rank.

## Run locally

Navigate to the entry directory and execute the main script using Python 3.

The entry's measured pooled faithful-backtest score is -32.200 overall, with order -18.765 and size -13.435, measured over ten reads in four splits.

```bash
cd /path/to/entry/directory
python3 main.py
