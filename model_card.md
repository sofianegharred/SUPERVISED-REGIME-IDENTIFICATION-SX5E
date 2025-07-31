# Model Card: SUPERVISED REGIME IDENTIFICATION SX5E

## Model Description

**Inputs**  
- Daily z-score-normalized features: V2X, iTraxx, EUR/USD, SX5E returns and their 1–5 day lags.

**Outputs**  
- Predicted regime label for next trading day.
- Regime rank (0 = highest expected return) and corresponding portfolio signal (long/short/flat).

**Architecture**  
1. **Gaussian Mixture Model**  
   - Components: 3, 5, or 7; full covariance; fixed random seed.  
   - Clusters training data into latent regimes.  
2. **Random Forest Classifier**  
   - 200 trees; max depth 5; fixed random seed.  
   - Trained to map features to GMM-derived regimes.

## Performance
- **Best Setup**: 3 regimes + RF(200, depth 5).
- **Out-of-Sample Sharpe**: ≈ 0.50 versus 0.02–0.05 for buy-and-hold.
- **Drawdown Profile**: Smoother equity curve with lower peak-to-trough losses.

## Limitations
- **Frequency**: Daily data may miss intraday regime shifts.
- **Static Training**: No retraining on rolling data—may drift over long horizons.
- **Clustering Sensitivity**: Results vary by GMM initialization and component count.

## Trade-Offs
- **Simplicity vs. Complexity**:  
  - Straightforward long/short/flat rule delivers solid Sharpe with minimal parameters.  
  - More complex approaches (e.g., deep learning) could boost accuracy at the cost of interpretability and greater overfitting risk.
