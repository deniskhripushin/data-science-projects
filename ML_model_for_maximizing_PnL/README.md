# Oil Region Profitability

A machine learning and business analysis project for selecting the most profitable region for oil well development.

Linear regression is used to predict oil reserves, while bootstrap simulation is applied to estimate expected profit, confidence intervals and the risk of financial loss.

## Objective

Select one of three regions for developing new oil wells.

The selected region should:

- provide the highest expected profit;
- have a loss probability below 2.5%;
- remain financially viable under the specified budget constraints.

## Data

The project uses three regional datasets, each containing 100,000 observations.

| Feature | Description |
|---|---|
| `id` | Unique well identifier |
| `f0` | Anonymized geological feature |
| `f1` | Anonymized geological feature |
| `f2` | Anonymized geological feature |
| `product` | Oil reserves in thousands of barrels |

The original datasets are not included in this repository.

## Business Parameters

| Parameter | Value |
|---|---:|
| Regional development budget | 10 billion RUB |
| Revenue per unit of product | 450,000 RUB |
| Explored wells per bootstrap sample | 500 |
| Wells selected for development | 200 |
| Maximum acceptable loss risk | 2.5% |

The minimum average reserve required for break-even development is approximately **111.11 thousand barrels per selected well**.

## Approach

For each region:

1. The identifier column was removed.
2. Data was divided into training and validation subsets using a 75/25 split.
3. A Linear Regression model was trained.
4. Oil reserves were predicted for the validation subset.
5. The 200 wells with the highest predicted reserves were selected.
6. Profit was calculated using the actual reserves of the selected wells.
7. Bootstrap simulation with 1,000 iterations was used to estimate expected profit, a 95% interval and loss probability.

## Model Results

| Region | Predicted average reserves | RMSE |
|---|---:|---:|
| Region 1 | 92.29 | 37.73 |
| Region 2 | 69.18 | 0.89 |
| Region 3 | 94.74 | 40.15 |

Region 2 has the lowest average predicted reserves but substantially lower prediction error. Regions 1 and 3 have larger predicted reserves but much higher uncertainty.

## Profit and Risk Analysis

Bootstrap results:

| Region | Mean profit | 95% quantile interval | Loss risk |
|---|---:|---:|---:|
| Region 1 | 409.49 million RUB | -110.16 to 946.23 million RUB | 6.0% |
| Region 2 | 481.92 million RUB | 102.40 to 850.93 million RUB | 0.6% |
| Region 3 | 377.63 million RUB | -154.60 to 853.91 million RUB | 6.8% |

Only Region 2 satisfies the maximum acceptable loss-risk requirement of 2.5%.

## Conclusion

Region 2 is the recommended location for oil well development.

Although its average predicted reserves are lower, it provides:

- the highest expected profit;
- the smallest model prediction error;
- a positive lower bound of the 95% bootstrap interval;
- the lowest estimated loss probability.

The analysis demonstrates that business decisions should consider prediction uncertainty and financial risk rather than relying only on average predicted reserves.

## Limitations

- The geological features are anonymized, which limits domain interpretation.
- Only Linear Regression was evaluated.
- `train_test_split` was used without a fixed `random_state`, so results may change when the notebook is rerun.
- The validation subset is used for both model evaluation and business simulation.
- Bootstrap results depend on the supplied cost and revenue assumptions.
- The analysis does not account for operational, infrastructure or environmental differences between regions.

Possible improvements include reproducible splitting, cross-validation, comparison with additional regression models and sensitivity analysis for oil prices and development costs.

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- SciPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## Repository Contents

- [`ML_model_for_maximizing_PnL.ipynb`](./ML_model_for_maximizing_PnL.ipynb) — model training, profit calculation and risk analysis;
- `README.md` — project description and results.
