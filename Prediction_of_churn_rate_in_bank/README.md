# Bank Customer Churn Prediction

A binary classification project for predicting whether a bank customer is likely to leave.

The project focuses on class imbalance, model comparison and improving recall for customers at risk of churn.

## Objective

Build a classification model that predicts the target variable `Exited`:

- `0` — customer remains with the bank;
- `1` — customer leaves the bank.

The target requirement is an F1-score of at least 0.59 on the test set.

## Data

The dataset contains 10,000 customer records.

| Feature | Description |
|---|---|
| `CreditScore` | Customer credit score |
| `Geography` | Country of residence |
| `Gender` | Customer gender |
| `Age` | Customer age |
| `Tenure` | Number of years as a bank customer |
| `Balance` | Account balance |
| `NumOfProducts` | Number of bank products used |
| `HasCrCard` | Credit card ownership indicator |
| `IsActiveMember` | Customer activity indicator |
| `EstimatedSalary` | Estimated salary |
| `Exited` | Target churn indicator |

Administrative columns such as row number, customer ID and surname were removed.

The original dataset is not included in this repository.

## Data Preparation

The preprocessing stage includes:

- removing non-predictive identifier columns;
- replacing missing `Tenure` values with the median value of 5;
- applying One-Hot Encoding to categorical features;
- scaling numerical features with `StandardScaler`;
- performing stratified train-validation-test splitting.

Dataset split:

| Subset | Records |
|---|---:|
| Training | 6,000 |
| Validation | 2,000 |
| Test | 2,000 |

## Class Imbalance

Approximately 20.4% of customers left the bank, producing a class ratio close to 1:4.

A constant model predicting that every customer would remain achieved an accuracy of 0.796. This demonstrates why accuracy alone is not suitable for evaluating the churn model.

The minority class was upsampled four times, producing an approximately balanced training dataset:

| Class | Share after upsampling |
|---|---:|
| Customer remains | 49.4% |
| Customer leaves | 50.6% |

## Models

The following classification algorithms were compared:

- Logistic Regression;
- Decision Tree Classifier;
- Random Forest Classifier.

The Random Forest produced the strongest validation F1-score after balancing the training data.

Final model parameters:

| Parameter | Value |
|---|---|
| Model | Random Forest Classifier |
| Number of trees | 100 |
| Maximum depth | 7 |
| Class weight | Balanced |
| Bootstrap | Enabled |
| Random state | 12345 |

## Results

Validation-set metrics:

| Metric | Value |
|---|---:|
| Accuracy | 0.822 |
| Precision | 0.546 |
| Recall | 0.757 |
| F1-score | 0.634 |

Test-set metrics:

| Metric | Value |
|---|---:|
| Precision | 0.520 |
| Recall | 0.715 |
| F1-score | 0.602 |

The final Random Forest model exceeded the required test F1-score of 0.59.

Recall of approximately 0.715 means that the model identifies about 71.5% of customers who actually leave the bank. Precision is lower, indicating that some customers are incorrectly classified as likely to leave.

## Conclusion

The initial models were strongly affected by class imbalance. Upsampling the minority class improved recall and F1-score, with Random Forest producing the strongest overall result.

The final model is more useful than the constant baseline for identifying customers at risk of churn. However, its moderate precision means that retention campaigns based on these predictions would also target some customers who were not going to leave.

## Limitations

- Missing `Tenure` values were replaced with a single median value.
- Upsampling and balanced class weights were used simultaneously, which may overcorrect class imbalance.
- Hyperparameters were selected using one validation split rather than cross-validation.
- The notebook calls `rec_prec_f1`, but its function definition is absent, so the notebook cannot be reproduced from a clean kernel without correction.
- The ROC curve is generated from an earlier Logistic Regression model rather than the final Random Forest.
- ROC-AUC should be calculated from predicted probabilities, not binary class predictions.
- No probability-threshold optimization or business cost analysis was performed.

Possible improvements include defining a reproducible evaluation function, calculating ROC-AUC correctly, tuning the classification threshold, using cross-validation and evaluating the financial value of retention actions.

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- Matplotlib
- Jupyter Notebook

## Repository Contents

- [`Prediction_of_churn_rate_in_bank.ipynb`](./Prediction_of_churn_rate_in_bank.ipynb) — preprocessing, class balancing, model training and evaluation;
- `README.md` — project description and results.
