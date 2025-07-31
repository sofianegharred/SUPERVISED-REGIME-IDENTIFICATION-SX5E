# Datasheet for Project_RAMA2.xlsx

## Motivation
- **Purpose**: Enable a regime-adaptive trading strategy on the EURO STOXX 50 by identifying and forecasting latent market regimes.
- **Creator**: Sofiane Gharred (Selwood Asset Management).

## Composition
- **Instances**: ~3 000 daily observations (2 500 for training; ~500 for testing).
- **Features**:
  - `sx5e_move`: daily percent change in SX5E.
  - V2X, iTraxx, EUR/USD returns.
  - Up to five lagged values for each feature.
- **Missing Data**: Columns with all missing values were dropped; no remaining full-column gaps.

## Collection Process
- **Sources**: Historical daily data for SX5E and related volatility/credit/currency series from commercial market-data providers.
- **Time Frame**: Covers all trading days for which the explanatory series and SX5E returns overlap (≈3 000 days).

## Preprocessing / Cleaning / Labeling
- **Cleaning**: Removed any columns comprised entirely of NaNs.
- **Normalization**: Z-score standardization applied to all explanatory series.
- **Labeling**: Computed `next_return` as the following day’s SX5E move; the final row (no next day) was dropped.
- **Raw Data**: Original unnormalized series are not included in the processed sheet.

## Uses
- **Intended Uses**:
  - Regime-adaptive forecasting and trading strategy development.
  - Risk management through regime-shift analysis.
- **Risks / Harms**:
  - Clustering depends on GMM assumptions; misclassification may hurt performance.
  - Not suited for intraday or high-frequency applications.
- **Mitigations**:
  - Thorough out-of-sample testing.
  - Sensitivity analysis over regime count.

## Distribution
- **Availability**: Hosted in this GitHub repository; no formal license—contact author for usage rights.

## Maintenance
- **Maintainer**: Sofiane Gharred (GitHub: @sofianegharred).
