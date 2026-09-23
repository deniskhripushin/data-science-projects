# Gold Recovery Prediction

A machine learning project for predicting gold recovery efficiency from ore-processing measurements.

The objective is to estimate recovery at two stages of the production process and evaluate predictions using a weighted symmetric mean absolute percentage error.

## Objective

Predict two target variables:

- `rougher.output.recovery` — recovery after flotation;
- `final.output.recovery` — recovery after final purification.

The final quality metric is:

**Final sMAPE = 0.25 × Rougher sMAPE + 0.75 × Final sMAPE**

Lower sMAPE values indicate better predictions.

## Data

The project uses three datasets:

| Dataset | Rows | Columns |
|---|---:|---:|
| Training data | 16,860 | 87 |
| Test data | 5,856 | 53 |
| Full process data | 22,716 | 87 |

The features describe different stages of ore processing:

- flotation;
- primary purification;
- secondary purification;
- final concentrate production.

They include feed characteristics, reagent quantities, equipment states and metal concentrations.

The original datasets are not included in this repository.

## Data Validation

The project verifies the provided flotation recovery calculation. The calculated and recorded values differ by an MAE of approximately `1.05 × 10⁻¹⁴`, confirming that the recovery formula was applied consistently.

The analysis also examines:

- missing values;
- features unavailable in the test set;
- gold, silver and lead concentrations;
- particle-size distributions;
- abnormal zero concentrations.

Rows containing missing values and selected anomalous observations were removed.

## Approach

Separate feature sets were prepared for the flotation and final recovery targets.

The following models were evaluated:

- Linear Regression;
- Polynomial Regression;
- Ridge Regression;
- Decision Tree Regressor.

The workflow includes cross-validation, hyperparameter tuning with `GridSearchCV` and comparison with a constant median baseline.

## Results

Reported test-set results from the notebook:

| Model | Final sMAPE |
|---|---:|
| Median baseline | 14.33 |
| Decision Tree Regressor | 14.93 |
| Linear Regression | 16.45 |
| Ridge Regression | 16.50 |
| Polynomial Regression | 28.28 |

The Decision Tree produced the best result among the trained models. However, it did not outperform the constant median baseline.

Additional comparison for final recovery:

| Model | R² | MAE |
|---|---:|---:|
| Ridge Regression | -0.527 | 9.68 |
| Median baseline | -0.139 | 7.79 |

These results indicate that the trained models do not provide a reliable improvement over a simple constant prediction.

## Conclusion

The project demonstrates data validation, industrial process analysis, feature preparation and regression model comparison.

However, the resulting models should not be considered production-ready because none of them outperformed the median baseline. Further feature engineering and validation improvements are required.

## Limitations

- Removing every row containing a missing value discards a substantial amount of data.
- Process observations are time-dependent, while dedicated time-series validation was not used.
- Test targets should be joined to test features by `date`; index-based selection may misalign observations.
- The notebook's written conclusion refers to Ridge as the best model, but the computed results show that Decision Tree has the lowest model sMAPE.
- No trained model outperformed the baseline.

Possible improvements include date-based target alignment, time-aware validation, careful missing-value imputation and gradient boosting models.

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- Matplotlib
- Jupyter Notebook

## Repository Contents

- [`ML_model_for_coefficient_prediction.ipynb`](./ML_model_for_coefficient_prediction.ipynb) — data analysis, preprocessing, model training and evaluation;
- `README.md` — project description and results.
