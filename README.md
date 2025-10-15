# Air-Temperature-Prediction
This repository contains a time-series deep learning project that predicts air temperature using historical sensor data and an LSTM-based model.

### Business Context and Objective
Accurate air temperature forecasting at short time horizons is valuable for many stakeholders — from agriculture and HVAC optimization to urban planning and environmental monitoring. This project explores whether a sequence model (LSTM) can learn temporal patterns in historical measurements to predict future air temperature.
The objective: build, train, and evaluate an LSTM model that predicts future air temperature from past time-series data, and report findings, limitations, and potential improvements.

### Key Steps in the Notebook
- Data ingestion & cleaning: load time-stamped sensor data, handle missing values, and resample/align timestamps as needed.
- Feature engineering: create lagged features / sliding windows for supervised learning (sequence-to-one setup).
- Scaling: scale features using MinMaxScaler applied only on training splits to avoid leakage.
- Modeling:
  1. Model 1: Single LSTM layer (64 units) + Dense output layer.
  2. Model 2: Two stacked LSTM layers (64 and 32 units) + Dense output layer, with hyperparameter tuning (batch size, learning rate, epochs).
- Optimizer: Adam with early stopping on validation loss.
- Evaluation: performance metrics include MSE, MAE, and R² on the test set.

### Results
- Model 1 — Single LSTM Layer
  1. MSE: 4.92
  2. MAE: 1.63
  3. R²: 0.18
- Model 2 — Two LSTM Layers (with tuning)
  1. MSE: 5.08
  2. MAE: 1.67
  3. R²: 0.12
 
### Summary 
Model 1 performed slightly better despite being simpler. The second model, though deeper and tuned, tended to overfit and did not improve generalization. The overall R² values remain low, indicating that the models fail to capture sufficient variance in temperature changes.

### Analysis and Discussion
Although the models were well-structured and trained correctly, their predictive power remained poor. The notebook’s conclusion highlights several reasons for this:
- Short training history: The available time window may not capture enough temporal diversity for the model to generalize.
- Noise and stationarity issues: The air temperature data may exhibit non-stationary behavior or high noise levels, complicating the learning process.
- Reconsider the target variable: because air temperature often contains strong trend and seasonal components, consider transforming the target to improve learnability and stability. Possible alternatives:
  1. Predict temperature change (ΔT) — model the difference between consecutive timestamps (differencing) to focus on short-term dynamics and remove trend.
  2. Predict daily/hourly anomalies — subtract a moving-average or climatology baseline first, then model the residual (anomaly) which is often more stationary.
  3. Predict aggregated statistics (e.g., daily mean, daily max/min) instead of instantaneous readings to reduce high-frequency noise.


