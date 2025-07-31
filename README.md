# SUPERVISED REGIME IDENTIFICATION SX5E

## Non-Technical Overview
This project develops a regime-adaptive trading strategy for the EURO STOXX 50 Index (SX5E). First, we **cluster** each trading day into latent market regimes using a Gaussian Mixture Model, then **rank** those regimes by their in-sample average next-day return. Next, a Random Forest is **trained** to predict tomorrow’s regime out of sample, and we **allocate** a €10 M long/short/flat portfolio based on the predicted regime, with the goal of maximizing out-of-sample Sharpe ratio.

## Data
- **Source file**: `Project_RAMA2.xlsx` (sheet `Processed2`)
- **Features**:
  - Daily z-score-normalized returns for V2X, iTraxx, EUR/USD, and the SX5E itself
  - Up to five daily lags for each series
- **Target**: `next_return` (tomorrow’s SX5E percent change)
- **Split**: First 2 500 rows for training, remaining ~500 rows for testing

## Model
1. **Unsupervised Regime Clustering**  
   - Fit GMMs with 3, 5, or 7 components on training features  
   - Assign each day to a cluster and rank clusters by mean next-day return  
2. **Supervised Regime Prediction**  
   - Train a Random Forest (`n_estimators=200`, `max_depth=5`) on the first 2 500 days to predict GMM labels  
   - Predict regimes out of sample without retraining  
3. **Regime-Adaptive Allocation**  
   - Top-ranked regimes: go long €10 M  
   - Bottom-ranked regimes: go short €10 M  
   - Middle regime (when odd count): flat  
4. **Backtest & Evaluation**  
   - Compare cumulative P&L versus buy-and-hold SX5E benchmark  

## Hyperparameter Optimization
- **Regime count**: tested 3, 5, and 7 regimes; 3 regimes gave the highest out-of-sample Sharpe (~0.5)  
- **Random Forest**: selected 200 trees and max depth 5 to balance predictive accuracy and overfitting risk

## Results
- **Sharpe Ratio** (3-regime strategy): ~0.50 versus 0.02–0.05 for buy-and-hold  
- **Drawdowns**: regime-adaptive approach meaningfully smoothed returns and reduced peak-to-trough losses

## Contact
GitHub: [sofianegharred/SUPERVISED-REGIME-IDENTIFICATION-SX5E](https://github.com/sofianegharred/SUPERVISED-REGIME-IDENTIFICATION-SX5E)
