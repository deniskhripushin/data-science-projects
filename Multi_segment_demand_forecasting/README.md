# Multi-Segment Demand Forecasting

Multi-series demand forecasting across store-item segments using the ETNA time-series framework.

The project compares baseline, statistical and machine learning models for forecasting daily product sales.

## Objective

Forecast daily demand for multiple store-item combinations over a 90-day horizon.

Each time series is represented as a separate segment:

```text
segment = store + item
```

For example:

```text
Store 3, item 27 → segment "3 + 27"
```

The experiment uses:

- stores 1, 2 and 3;
- 50 products per store;
- 150 time-series segments;
- daily observations;
- a 90-day forecasting horizon.

## Dataset

The project uses data from the Kaggle competition:

[Store Item Demand Forecasting Challenge](https://www.kaggle.com/competitions/demand-forecasting-kernels-only)

Expected training columns:

| Column | Description |
|---|---|
| `date` | Observation date |
| `store` | Store identifier |
| `item` | Product identifier |
| `sales` | Number of items sold |

The data files are not stored in this repository.

After downloading the competition data, rename the files:

```text
train.csv → HW_train.csv
test.csv  → HW_test.csv
```

Place them in the project directory before running the notebook.

## Data Preparation

The original training dataset contains:

```text
913,000 observations
10 stores
50 products
```

To reduce training time, the experiment uses stores 1, 2 and 3, which represent 30% of the complete dataset:

```text
273,900 observations
150 store-item segments
```

The data is converted into ETNA's wide time-series format using `TSDataset`.

Store and item identifiers are included as known future regressors.

## Feature Engineering

The CatBoost model uses several groups of time-series features.

### Lag Features

```text
1, 2, 3, 7, 14, 21, 28, 56, 91, 182 and 364 days
```

These features represent short-term, weekly, monthly, quarterly and yearly sales history.

### Rolling Statistics

Rolling mean features are calculated using windows of:

```text
7, 14 and 28 days
```

### Calendar Features

The following date characteristics are generated:

- day of the week;
- day of the month;
- week of the month;
- month of the year;
- year.

## Models

The following models are compared:

### Naive Model

Uses previous observations as the forecast and provides a simple baseline.

### Linear Model

An `ElasticPerSegmentModel` is trained separately for every segment using:

- one-day lag;
- seven-day rolling mean;
- feature standardization;
- autoregressive forecasting.

### AutoARIMA

An individual statistical ARIMA model is trained for every segment.

### Prophet

Prophet is configured with:

- linear trend;
- weekly seasonality;
- yearly seasonality;
- multiplicative seasonal effects;
- `changepoint_prior_scale = 0.1`;
- `seasonality_prior_scale = 10`.

### CatBoost

A single `CatBoostMultiSegmentModel` is trained across all segments.

Configuration:

```text
Iterations:    500
Tree depth:    6
Learning rate: 0.05
Loss function: RMSE
Random seed:   42
```

The model combines information from all store-item series and uses lag, rolling and calendar features.

## Evaluation

Forecast quality is evaluated using SMAPE:

```text
Symmetric Mean Absolute Percentage Error
```

The metric is calculated independently for each segment and then averaged across all 150 segments.

Lower SMAPE values indicate better forecasting quality.

## Results

| Model | Average SMAPE |
|---|---:|
| Naive | 27.94 |
| Linear model | 19.22 |
| AutoARIMA | 19.22 |
| Prophet | 12.52 |
| **CatBoost** | **11.87** |

CatBoost achieved the best result and was the only model to reach the target of:

```text
SMAPE < 12%
```

Prophet produced a competitive result but remained slightly above the target threshold.

The linear model and AutoARIMA showed nearly identical performance, while the naive approach produced the highest forecasting error.

## Backtesting

The best CatBoost pipeline is additionally evaluated using expanding-window backtesting:

```text
Number of folds: 5
Forecast horizon: 90 days
```

Backtesting measures model quality across several consecutive historical periods instead of relying on only one holdout interval.

## Conclusion

CatBoost produced the best demand forecast because it can model nonlinear relationships between:

- recent sales;
- weekly and long-term lags;
- rolling demand statistics;
- calendar characteristics;
- store and product identifiers.

Training one global model across all segments also allows information to be shared between different store-item series.

## Technologies

- Python
- pandas and NumPy
- ETNA
- CatBoost
- Prophet
- AutoARIMA
- scikit-learn
- Jupyter Notebook

## Notebook

[Open the forecasting notebook](./multi_segment_demand_forecasting.ipynb)

## Running the Project

Install the required libraries:

```bash
pip install pandas numpy scikit-learn catboost prophet pmdarima etna jupyter
```

Place the data files in the project directory:

```text
Multi_segment_demand_forecasting/
├── multi_segment_demand_forecasting.ipynb
├── HW_train.csv
└── HW_test.csv
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open `multi_segment_demand_forecasting.ipynb` and run the cells in order.

Some experiments, particularly autoregressive pipelines and multi-fold backtesting, may require several minutes to complete.

## Limitations

- Only three of the ten available stores are used.
- The forecasting horizon is fixed at 90 days.
- The experiment does not include prices, promotions, holidays or stock availability.
- Hyperparameter optimization is limited.
- Model quality may differ between individual store-item segments.
- The notebook evaluates models offline and does not generate a complete Kaggle submission.
