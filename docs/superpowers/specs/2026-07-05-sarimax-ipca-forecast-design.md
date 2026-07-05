# SARIMAX IPCA Forecast — Design

## Context

`ipca_forecast.ipynb` forecasts monthly Brazilian IPCA inflation with XGBoost + Optuna
tuning on ~25 engineered tabular features (lags, rolling means/vol, interactions).
Test-set performance: RMSE 0.364, MAE 0.265, R² 0.243. The concern: XGBoost is a
generic tabular regressor and doesn't natively model the sequential/autoregressive
structure of a time series, which may explain the modest R² on a small
(~300-400 monthly observations) sample.

## Goal

Replace XGBoost with SARIMAX, a model built for autoregressive time-series structure,
in a **new, separate notebook**. `ipca_forecast.ipynb` stays untouched as the existing
XGBoost reference.

## Data pipeline

Reuse the existing data-collection code from `ipca_forecast.ipynb`:
- SGS series (IPCA, IPCA_EX1, IPA-DI, IGP-M, INCC, INPC, PIB_mensal, SELIC, Câmbio, CDI)
- FRED commodities (Brent oil, soy, corn, beef)
- HP filter for `PIB_hiato`
- 1-month shift on all non-IPCA columns to avoid look-ahead bias

## Exogenous features (trimmed set)

SARIMAX's own AR/MA terms already model IPCA's autocorrelation, so IPCA's own
lags/rolling trend/volatility features (used for XGBoost) are dropped to avoid
redundancy and multicollinearity on a small sample. Exogenous regressors:

1. Câmbio (lagged)
2. Selic real ex-ante (`SELIC - IPCA_acumulado_12_meses`)
3. Hiato do produto (HP filter)
4. IPA-DI
5. Índice de commodities

5 regressors against ~300-400 observations.

## Model

`pmdarima.auto_arima(y, exogenous=X, seasonal=True, m=12, stepwise=True)` —
stepwise AIC search over `(p,d,q)(P,D,Q,12)`. `pmdarima` is a new dependency
(not currently installed in `.venv`); it wraps a `statsmodels` SARIMAX under the
hood, so the fitted model is inspectable via `.summary()` like any statsmodels
result.

## Train/test split

Same chronological 80/20 split as `ipca_forecast.ipynb`, so metrics are directly
comparable across the two notebooks.

## Evaluation

Same metrics as the XGBoost notebook: RMSE, MAE, R², plus the same two naive
baselines already used there (historical mean of train, persistence/random walk)
for a consistent sanity check.

## Output

- Metrics table (SARIMAX vs. mean baseline vs. persistence baseline)
- Real-vs-predicted plot styled like `grafico4_previsoes_vs_real.png`, saved as
  e.g. `grafico5_sarimax_previsoes.png`

## Out of scope

- No changes to `ipca_forecast.ipynb`.
- No ensembling/blending of SARIMAX and XGBoost predictions.
- No VAR/VECM or state-space alternatives (considered and deferred in favor of
  SARIMAX for this pass).
