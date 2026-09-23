# House Prices AutoML

Machine learning solution for the Kaggle
[House Prices: Advanced Regression Techniques](https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques)
competition.

## Objective

Predict residential property prices using tabular housing characteristics.

## Approach

- Exploratory data analysis
- Missing-value analysis
- Feature preprocessing
- Train/validation split
- LightAutoML model training
- Comparison of `TabularAutoML` and `TabularUtilizedAutoML`
- Evaluation using RMSE

## Technologies

- Python
- pandas and NumPy
- scikit-learn
- LightAutoML
- Matplotlib and Seaborn
- ydata-profiling

## Data

Competition data is not stored in this repository.

Download it from Kaggle:

```bash
kaggle competitions download \
  -c house-prices-advanced-regression-techniques
```

Place `train.csv`, `test.csv` and `sample_submission.csv` in this
directory before running the notebook.

## Notebook

[Open the analysis](./house_prices_automl.ipynb)
