# Taxi Demand Forecasting

## Project Overview

This project develops a machine learning model for forecasting the number of taxi orders for the next hour.

Accurate short-term demand forecasts can help a taxi company allocate drivers more efficiently during periods of high demand.

## Objective

Build a regression model that predicts the number of taxi orders for the next hour.

The target quality requirement is:

- **RMSE no greater than 48 orders**

## Dataset

The dataset contains historical taxi demand recorded at 10-minute intervals.

| Column | Description |
|---|---|
| `datetime` | Date and time of observation |
| `num_orders` | Number of taxi orders |

The original dataset contains **26,496 observations** covering the period from March to August 2018.

For model training, the observations were resampled into hourly intervals by summing the number of orders within each hour. After resampling, the dataset contained **4,416 hourly observations**.

The dataset itself is not included in this repository.

## Time-Series Analysis

The analysis included:

- examination of the demand distribution;
- hourly resampling of the original observations;
- decomposition into trend, seasonality and residual components;
- analysis of changes in demand over time;
- Augmented Dickey-Fuller test for stationarity.

The time series demonstrates recurring intraday patterns and an overall increase in demand toward the end of the observed period.

The Augmented Dickey-Fuller test produced a p-value of approximately **0.029**, indicating that the unit-root hypothesis can be rejected at the 5% significance level.

## Feature Engineering

The following features were generated from the time index:

- year;
- month;
- day;
- day of the week;
- hour of the day.

To represent the temporal structure of the series, additional features were created:

- lag values for the previous 24 hours;
- rolling mean with a 20-hour window.

The rolling mean was shifted by one observation so that the current target value was not used when generating predictors.

## Validation Strategy

The observations were divided chronologically without shuffling:

- **90%** for model training;
- **10%** for final testing.

Hyperparameter selection was performed using `TimeSeriesSplit`, which preserves the temporal order of observations and prevents future data from being used to predict the past.

## Models

The following regression models were evaluated:

- Linear Regression;
- Random Forest Regressor;
- CatBoost Regressor;
- LightGBM Regressor;
- XGBoost Regressor.

Hyperparameters for the ensemble models were selected using time-series cross-validation.

## Results

Final model performance on the test sample:

| Model | Test RMSE |
|---|---:|
| LightGBM | **39.24** |
| CatBoost | 40.38 |
| Random Forest | 44.41 |
| Linear Regression | 48.86 |

LightGBM achieved the lowest test RMSE. CatBoost and Random Forest also satisfied the target requirement, while Linear Regression slightly exceeded the maximum acceptable RMSE of 48.

## Conclusion

The best result was achieved by **LightGBM**, with a test RMSE of approximately **39.24 orders**.

This result satisfies the project requirement and demonstrates that lag features, rolling statistics and calendar variables provide useful information for short-term taxi demand forecasting.

CatBoost produced a comparable result and may be a practical alternative when model training and prediction speed are also important.

## Limitations

- The dataset covers only six months and therefore does not represent annual seasonality.
- Model performance was evaluated on a single final holdout period.
- The final LightGBM model shows a substantial difference between training and test RMSE, which indicates possible overfitting.
- Some experiments used the test sample as an evaluation set during training. A separate validation period should be used in a production workflow.
- A seasonal naive forecast should be added as a baseline for comparison.
- Training-time values recorded in the notebook are not measured consistently and should not be used for a strict performance comparison.

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- statsmodels
- CatBoost
- LightGBM
- XGBoost
- Matplotlib
- Jupyter Notebook

## Repository Contents

- `Prediction_of_taxi_orders.ipynb` — data analysis, feature engineering, model training and evaluation
- `README.md` — project description and results
