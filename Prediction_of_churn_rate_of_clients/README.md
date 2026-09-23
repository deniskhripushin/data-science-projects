# Telecom Customer Churn Prediction

## Project Overview

This project develops a machine learning model for predicting customer churn at a telecommunications company.

Customers identified as being at high risk of leaving can be offered promotional codes, discounted tariff plans or additional services as part of a retention campaign.

## Objective

Build a binary classification model that predicts whether a customer will terminate their contract.

The target variable is:

- `0` — the customer remains with the company;
- `1` — the customer has terminated the contract.

The principal evaluation metric is **ROC AUC**.

## Datasets

Customer information is distributed across four related datasets.

| Dataset | Records | Description |
|---|---:|---|
| Contract | 7,043 | Contract dates, billing method and customer charges |
| Personal | 7,043 | Demographic and household information |
| Internet | 5,517 | Internet connection type and additional services |
| Phone | 6,361 | Telephone-service information |

The datasets were joined using the unique `customerID` field.

The source data is not included in this repository.

## Target Distribution

| Customer Status | Records | Share |
|---|---:|---:|
| Active | 5,174 | 73.46% |
| Churned | 1,869 | 26.54% |

The target is moderately imbalanced, with approximately one churned customer for every three active customers.

## Data Preprocessing

The following preprocessing steps were performed:

- four source tables were joined by customer identifier;
- missing service records were interpreted as the absence of the corresponding service;
- `BeginDate` was converted to a date;
- `TotalCharges` was converted to a numeric variable;
- 11 missing `TotalCharges` values were replaced with zero;
- binary categorical variables were converted to `0` and `1`;
- multi-category variables were encoded using one-hot encoding;
- a binary churn target was created from `EndDate`;
- customer duration was calculated relative to the observation date;
- raw contract dates were removed before model training.

The resulting dataset contained 7,043 customer records.

## Exploratory Analysis

The analysis showed that:

- month-to-month contracts were the most common;
- fibre-optic internet was more common than DSL;
- electronic check was the most frequently used payment method;
- many customers did not use online security or technical support;
- paperless billing was preferred by most customers;
- recently connected customers represented an important churn group.

## Features

The models used customer characteristics including:

- customer duration;
- monthly charges;
- total charges;
- contract type;
- internet connection type;
- payment method;
- paperless billing;
- online security;
- technical support;
- additional telephone lines;
- streaming services;
- demographic and household attributes.

The most important features in the tree-based models were:

1. total charges;
2. customer duration;
3. monthly charges;
4. two-year contract;
5. fibre-optic internet;
6. one-year contract;
7. electronic-check payment;
8. online security and technical support.

## Validation Strategy

The dataset was randomly divided into:

- **70% training data**;
- **30% test data**.

Hyperparameter selection was performed using five-fold cross-validation and ROC AUC as the optimisation metric.

## Models

The following models were evaluated:

- Logistic Regression;
- LightGBM;
- Random Forest;
- CatBoost.

Class weighting was applied to the Random Forest model to reduce the effect of target imbalance.

## Results

The notebook contains several experiments using different feature sets and validation procedures.

| Model | Evaluation | ROC AUC |
|---|---|---:|
| Logistic Regression | 5-fold cross-validation | 0.800 |
| LightGBM | Test sample, initial model | **0.905** |
| LightGBM | 5-fold cross-validation, tuned model | 0.842 |
| Random Forest | 5-fold cross-validation, tuned model | 0.850 |
| Random Forest | Test sample | 0.844 |
| CatBoost | 5-fold cross-validation, tuned model | 0.855 |
| CatBoost | Test sample, reduced feature set | 0.855 |

The tuned Random Forest produced a test accuracy of approximately **0.760**, while the final CatBoost model achieved approximately **0.803**.

The highest directly reported test ROC AUC among the primary experiments was obtained by the initial LightGBM model. CatBoost also demonstrated strong performance, although its initial ROC AUC of approximately 0.932 should be interpreted cautiously because the test sample was supplied as an evaluation set during training.

## Conclusion

Gradient-boosting models produced the strongest churn-ranking performance.

Customer duration, accumulated and monthly charges, contract length, internet connection type and payment method were the most informative predictors.

The results indicate that customers with short-term contracts, fibre-optic service and electronic-check payments may deserve additional attention from the retention team. These associations should be validated through a clean final model and business experiments before being used operationally.

## Business Application

The model can be used to assign customers a churn probability and prioritise retention actions.

A production workflow could include:

- generating churn scores regularly;
- selecting an intervention threshold based on campaign costs;
- offering targeted discounts or service upgrades;
- monitoring retention after an offer;
- comparing campaign results against a control group.

## Limitations

- The experiment does not retain a completely untouched final validation set.
- Some model selection decisions were informed by test-set performance.
- CatBoost used the test sample as an evaluation set in one experiment.
- `LGBMRegressor` was used for a classification problem; `LGBMClassifier` would be more appropriate.
- One Logistic Regression ROC AUC calculation used predicted classes instead of probabilities.
- The train-test split was not explicitly stratified by the churn target.
- The calculation and business meaning of customer duration require clarification for customers who had already churned.
- The notebook's final narrative reports a LightGBM ROC AUC of 0.893, but that exact value is not present in the saved outputs.
- Precision, recall, PR AUC, probability calibration and business costs were not evaluated.
- Model performance should be confirmed with a consistent preprocessing pipeline and a new holdout sample.

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- LightGBM
- CatBoost
- Matplotlib
- Seaborn
- Jupyter Notebook

## Repository Contents

- `Prediction_of_churn_rate_of_clients.ipynb` — data preparation, exploratory analysis, model training and evaluation
- `README.md` — project description, results and limitations
