# Used Car Price Prediction

A machine learning project for estimating the market value of used cars from vehicle characteristics.

The project compares several regression algorithms with respect to prediction quality, training time and inference speed.

## Objective

Build a regression model that predicts a vehicle's price using its technical specifications and listing information.

The model should provide:

- low prediction error;
- reasonable training time;
- fast inference;
- stable performance on unseen data.

Model quality is evaluated using Root Mean Squared Error (RMSE). Lower RMSE values indicate better predictions.

## Data

The original dataset contains 354,369 used-car listings and 16 columns.

The selected predictive features include:

| Feature | Description |
|---|---|
| `VehicleType` | Vehicle body type |
| `Gearbox` | Transmission type |
| `Power` | Engine power |
| `Kilometer` | Vehicle mileage |
| `FuelType` | Fuel type |
| `Brand` | Vehicle manufacturer |
| `NotRepaired` | Whether the vehicle was previously repaired |
| `RegistrationYear` | Vehicle registration year |
| `Model` | Vehicle model |
| `Price` | Target vehicle price |

Administrative and non-predictive columns such as crawl dates, postal codes and image counts were removed.

After preprocessing and filtering, 237,573 observations remained.

The original dataset is not included in this repository.

## Data Preparation

The preprocessing stage includes:

- selecting relevant features;
- removing implausible prices, registration years and engine power values;
- handling missing categorical values;
- filling missing vehicle and fuel types using brand-level information;
- converting binary categorical features;
- reducing numerical data types;
- preparing one-hot and ordinal encoded feature sets;
- splitting the data into training and test subsets using a 75/25 ratio.

Categorical features were encoded differently depending on model requirements:

- One-Hot Encoding for linear models;
- Ordinal Encoding for tree-based models;
- native categorical features for CatBoost.

## Models

The following regression algorithms were evaluated:

- Linear Regression;
- Ridge Regression;
- Decision Tree Regressor;
- CatBoost Regressor;
- LightGBM Regressor.

Hyperparameters were selected using cross-validation and `GridSearchCV`.

## Results

Test-set performance:

| Model | Test RMSE |
|---|---:|
| LightGBM Regressor | 1,156.48 |
| CatBoost Regressor | 1,187.44 |
| Decision Tree Regressor | 1,328.64 |
| Linear Regression | 1,716.11 |
| Ridge Regression | 1,716.17 |

LightGBM achieved the lowest test RMSE.

Selected LightGBM parameters:

| Parameter | Value |
|---|---:|
| `learning_rate` | 0.3 |
| `num_leaves` | 100 |
| `random_state` | 12345 |

Selected Decision Tree maximum depth:

| Encoding | Maximum depth |
|---|---:|
| One-Hot Encoding | 15 |
| Ordinal Encoding | 13 |

Gradient boosting models substantially outperformed the linear models and the standalone decision tree.

## Performance

The notebook also compares training and prediction time.

Main observations:

- Linear Regression trained relatively quickly but produced the highest error.
- CatBoost achieved strong predictive quality but required more time during hyperparameter search.
- LightGBM combined the lowest RMSE with fast prediction.
- Decision Tree was faster but less accurate than the boosting models.

Measured execution times depend on hardware, library versions and the selected hyperparameter grid, so they should be interpreted as approximate comparisons rather than universal benchmarks.

## Conclusion

LightGBM was selected as the best model because it provided the strongest balance between:

- prediction quality;
- training time;
- inference speed.

CatBoost produced a similar RMSE but required more computation during model selection. Linear Regression and Ridge Regression were faster and simpler but were unable to capture the nonlinear relationships in the vehicle data.

## Limitations

- Data cleaning reduced the dataset from 354,369 to 237,573 observations.
- Some missing values were filled using heuristic rules.
- Ordinal encoding introduces an artificial numerical order between categories.
- Encoders were fitted before the train-test split rather than inside a validation pipeline.
- A simple baseline model such as `DummyRegressor` was not included.
- Listing dates were not used for temporal validation.
- Reported execution times are specific to the environment in which the notebook was run.
- The model was evaluated only on historical listing data and was not deployed.

Possible improvements include pipeline-based preprocessing, baseline comparison, time-aware validation, feature-importance analysis and explainability with SHAP values.

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- CatBoost
- LightGBM
- Matplotlib
- Jupyter Notebook

## Repository Contents

- [`Model_to_define_car_price.ipynb`](./Model_to_define_car_price.ipynb) — data preprocessing, model training, tuning and evaluation;
- `README.md` — project description and results.
