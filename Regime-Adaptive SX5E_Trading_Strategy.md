# SUPERVISED-REGIME-IDENTIFICATION-SX5E
SUPERVISED REGIME IDENTIFICATION SX5E
# Regime-Adaptive SX5E Trading Strategy

A concise summary of our regime‐adaptive trading project for the EURO STOXX 50 Index (SX5E). All code is contained in the accompanying Jupyter Notebook—this README explains the high‐level approach, data, and key findings.

---

## Project Overview

We build a market‐regime‐aware strategy that:

1. **Clusters** each trading day (using GMM) into a fixed number of latent regimes (3, 5, or 7) based on normalized explanatory features (e.g., V2X, ITRAXX, EURUSD moves and their lags).  
2. **Ranks** those regimes by in‐sample average next‐day SX5E return.  
3. **Trains** a random forest classifier to predict tomorrow’s regime out‐of‐sample.  
4. **Allocates** a €10 M notional long/short portfolio:  
   - If predicted regime is in the top half → go long  
   - If predicted regime is in the bottom half → go short  
   - If exactly in the middle (when odd count) → stay flat  
5. **Backtests** on the last ~500 days and compares vs. a buy‐and‐hold benchmark.  

Our goal: maximize out‐of‐sample Sharpe ratio.

---

## Data

- **File**: `Project_RAMA2.xlsx`  
- **Sheet**: `Processed2` (all explanatory variables are already z‐score normalized; includes up to five daily lags for each series).  
- **Key Series**:  
  - `sx5e_move` (daily ES 50 % change)  
  - V2X, ITRAXX moves, EUR/USD moves, QW5A moves, plus their lagged values  
  - Computed `next_return` = tomorrow’s SX5E move (decimal)  

After dropping any column that is fully NaN, we keep all remaining features. The first 2,500 rows (chronologically sorted) form the training set; the subsequent ~500 rows form the test set.

---

## Methodology

1. **Unsupervised Regime Clustering (GMM)**  
   - Fit a Gaussian Mixture Model on the training set features using **3, 5, and 7 components**.  
   - Assign each training day to a cluster label; compute each cluster’s average next‐day return.  
   - Rank clusters from 0 (highest mean return) to (n_regimes − 1) (lowest).

2. **Supervised Regime Prediction (Random Forest) **  
   - Train a `Random Forest` model on the first 2,500 days’ features → cluster labels.  
   - Evaluate in‐sample accuracy (as a sanity check).  
   - Predict cluster labels (and corresponding rank) on test days without ever retraining on test data.

3. **Regime‐Adaptive Signal**  
   - On each test day, let **rank < middle** = long €10 M; **rank > middle** = short €10 M; **rank = middle** = flat (when n_regimes is odd).  
   - Compute daily PnL: `position × next_return` (with €10 M notional).  
   - Compare cumulative PnL vs. buy‐and‐hold (constant €10 M long) over the same test days.

4. **Performance Metrics**  
   - **Total Return** (test period cumulative PnL).  
   - **Annualized Return/Volatility** (252 trading days).  
   - **Sharpe Ratio** (annualized, no risk‐free).  
   - **Max Drawdown** (peak‐to‐trough on the cumulative wealth curve).  


