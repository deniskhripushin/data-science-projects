# Store Demand Forecasting

Time-series forecasting of daily product demand using Prophet, AutoARIMA, SARIMAX and TBATS.

The project compares several statistical forecasting approaches and evaluates how seasonal patterns, calendar features and target transformations affect forecast quality.

## Objective

Forecast daily sales for:

```text
Store: 1
Item:  1
```

The final 365 observations are used as the holdout period, representing a one-year forecasting horizon.

## Dataset

The project uses data from the Kaggle competition:

[Store Item Demand Forecasting Challenge](https://www.kaggle.com/competitions/demand-forecasting-kernels-only)

The source dataset contains daily sales for multiple products and stores.

Expected columns:

| Column | Description |
|---|---|
| `date` | Observation date |
| `store` | Store identifier |
| `item` | Product identifier |
| `sales` | Number of items sold |

The data files are not stored in this repository. Download the competition data and place `train.csv` in the project directory before running the notebook.

## Data Preparation

The notebook performs the following steps:

1. Loads the daily sales history.
2. Selects `store = 1` and `item = 1`.
3. Converts the `date` column into a time index.
4. Reserves the last 365 days for testing.
5. Examines weekly and yearly seasonal patterns.
6. Creates calendar-based features for models with external regressors.

Calendar features include:

- weekly sine and cosine components;
- monthly sine and cosine components;
- yearly sine and cosine components;
- weekend indicator;
- beginning-of-month indicator;
- end-of-month indicator.

## Models

The following forecasting approaches are compared:

### Prophet

Several Prophet configurations are evaluated:

- basic Prophet;
- Prophet with US holidays;
- Prophet with Box-Cox transformation;
- tuned Prophet with additive and multiplicative seasonality.

The tuning process tests different values of:

- `changepoint_prior_scale`;
- `seasonality_prior_scale`;
- `seasonality_mode`;
- yearly Fourier order.

### AutoARIMA

Two configurations are evaluated:

- seasonal AutoARIMA;
- seasonal AutoARIMA with calendar regressors.

The model uses a weekly seasonal period of seven days.

### SARIMAX

Two SARIMAX models are evaluated:

- SARIMAX without external regressors;
- SARIMAX with calendar features.

### TBATS

TBATS is configured to model multiple seasonal periods:

```text
Weekly seasonality: 7 days
Yearly seasonality: 365.25 days
```

## Evaluation

Forecast quality is measured using:

- **MSE** — Mean Squared Error;
- **MAE** — Mean Absolute Error;
- **MAPE** — Mean Absolute Percentage Error.

MAPE is used as the primary metric for selecting the best model.

## Results

| Model | MSE | MAE | MAPE |
|---|---:|---:|---:|
| Prophet | 24.681 | 4.032 | 22.23% |
| Prophet with US holidays | 25.022 | 4.068 | 22.48% |
| Prophet with Box-Cox | 24.111 | 3.987 | 21.68% |
| AutoARIMA | 57.184 | 5.920 | 27.38% |
| AutoARIMA with calendar features | 25.151 | 3.966 | 20.22% |
| SARIMAX | 70.672 | 6.641 | 28.85% |
| SARIMAX with calendar features | 24.962 | 3.977 | 20.08% |
| **TBATS** | **23.633** | **3.856** | **19.79%** |

Additional Prophet tuning reduced MAPE to approximately:

- tuned Prophet: `20.84%`;
- tuned Prophet with Box-Cox: `20.68%`.

TBATS produced the lowest MSE, MAE and MAPE. It was the only model in the final comparison to achieve MAPE below 20%.

## Conclusion

Models with explicit calendar features substantially outperformed their basic AutoARIMA and SARIMAX counterparts.

Adding US holidays did not improve Prophet because the selected sales series appears to be driven more strongly by recurring seasonal patterns than by the holiday calendar.

TBATS achieved the best result by modeling weekly and yearly seasonality simultaneously.

## Technologies

- Python
- pandas and NumPy
- Matplotlib
- scikit-learn
- Prophet
- pmdarima
- statsmodels
- TBATS
- SciPy
- Jupyter Notebook

## Notebook

[Open the forecasting notebook](./store_demand_forecasting.ipynb)

## Running the Project

Install the required libraries:

```bash
pip install pandas numpy matplotlib scikit-learn scipy prophet pmdarima statsmodels tbats jupyter
```

Place `train.csv` in the project directory:

```text
Store_demand_forecasting/
├── store_demand_forecasting.ipynb
└── train.csv
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open `store_demand_forecasting.ipynb` and run the cells in order.

## Limitations

- Only one store-item combination is modeled.
- Hyperparameters are selected using the same holdout period used for final comparison.
- A separate validation period or rolling time-series cross-validation would provide a more reliable estimate.
- Some SARIMAX configurations produce convergence warnings.
- External factors such as prices, promotions, stock availability and economic conditions are not available.
- The notebook does not generate a submission for the complete Kaggle test set.
