# Carry Premium Project

This project is a bond portfolio optimization algorithm focused on capturing fixed-income carry premiums by dynamically adjusting bond ETF allocations.

## Purpose

The goal is to maximize returns by constructing a bond portfolio with high yield spreads while maintaining diversification, capturing a fixed income carry premium across economic cycles.

## Approach

The algorithm optimizes bond allocations among various ETFs by prioritizing higher-yielding bonds and periodically rebalancing based on current yields, thus targeting yield maximization through a carry strategy.

## Method

1. **Portfolio Initialization**: The algorithm sets up a bond portfolio using ETFs (TLT, LQD, JNK, EMB) and a fixed cash allocation.
2. **Monthly Rebalancing**: At the start of each month, the portfolio weights are recalculated based on each ETF’s approximate yield, achieved using historical dividend data as a yield proxy.
3. **Weight Allocation**: Portfolio weights are dynamically assigned, favoring higher yields, and adjusted to preserve diversification.

## Result

This approach aims to enhance the portfolio’s carry premium and risk-adjusted return, with weights reallocated monthly to capture changes in bond yields, providing exposure to diverse fixed-income assets.

---
