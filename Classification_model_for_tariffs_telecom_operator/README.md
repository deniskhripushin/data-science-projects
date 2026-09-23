# Telecom Tariff Recommendation

A machine learning project for recommending one of two mobile plans based on a customer's monthly service usage.

The objective is to build a binary classification model that predicts whether a customer should use the **Ultra** plan instead of the **Smart** plan.

## Objective

Train and evaluate a classification model with an accuracy of at least 0.75.

The target variable is:

- `is_ultra = 1` — Ultra plan;
- `is_ultra = 0` — Smart plan.

## Dataset

The dataset contains 3,214 customer records with the following features:

| Feature | Description |
|---|---|
| `calls` | Number of calls |
| `minutes` | Total call duration in minutes |
| `messages` | Number of text messages |
| `mb_used` | Mobile internet traffic in megabytes |
| `is_ultra` | Target plan indicator |

The data had already been preprocessed before the analysis.

The original dataset is not included in this repository.

## Approach

The data was divided into three subsets:

- 60% training data;
- 20% validation data;
- 20% test data.

Three classification algorithms were evaluated:

- Logistic Regression;
- Decision Tree;
- Random Forest.

Model parameters were selected using the validation set. Final model quality was measured on the test set using accuracy, precision, recall and F1-score.

## Results

| Model | Validation accuracy | Test accuracy |
|---|---:|---:|
| Logistic Regression | 0.722 | 0.708 |
| Decision Tree | 0.832 | 0.806 |
| Random Forest | 0.824 | 0.806 |

The Decision Tree used a maximum depth of 7.  
The Random Forest used 54 estimators.

Additional Random Forest test metrics:

| Metric | Value |
|---|---:|
| Accuracy | 0.806 |
| Precision | 0.735 |
| Recall | 0.557 |
| F1-score | 0.633 |

Both tree-based models exceeded the target accuracy of 0.75. The Decision Tree produced the highest validation accuracy, while the Decision Tree and Random Forest achieved the same accuracy on the test set.

## Limitations

Hyperparameters were selected using a single validation split rather than cross-validation. The relatively lower recall indicates that the model misses some customers who should be assigned to the Ultra plan.

Possible improvements include:

- stratified data splitting;
- cross-validation;
- broader hyperparameter optimization;
- class-imbalance analysis;
- comparison with gradient boosting models;
- evaluation using ROC-AUC and a confusion matrix.

## Technologies

- Python
- pandas
- scikit-learn
- Jupyter Notebook

## Repository Contents

- [`Classification_model_for_tariffs_telecom_operator.ipynb`](./Classification_model_for_tariffs_telecom_operator.ipynb) — data analysis, model training and evaluation;
- `README.md` — project description and results.
