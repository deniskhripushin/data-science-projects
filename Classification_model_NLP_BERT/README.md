# Toxic Comment Classification

An NLP project for identifying toxic user comments and supporting automated content moderation.

Text features are extracted with TF-IDF and evaluated with several binary classification models.

## Objective

Build a model that classifies comments as:

- `0` — non-toxic;
- `1` — toxic.

The target requirement is an F1-score of at least 0.75.

## Dataset

The dataset contains 159,571 English-language comments:

| Class | Records |
|---|---:|
| Non-toxic | 143,346 |
| Toxic | 16,225 |

The class ratio is approximately 8.83:1, indicating significant class imbalance.

The original dataset is not included in this repository.

## Approach

The project includes:

- text normalization and removal of non-alphabetic characters;
- English stop-word removal;
- TF-IDF vectorization;
- 60/20/20 train-validation-test split;
- class weighting and downsampling experiments;
- three-fold cross-validation;
- hyperparameter tuning with `GridSearchCV`.

The following classifiers were compared:

- Logistic Regression;
- SGDClassifier;
- Decision Tree;
- CatBoost.

Class weighting was selected instead of downsampling because it produced more stable validation results.

## Results

| Model | CV F1 | Validation F1 | Test F1 |
|---|---:|---:|---:|
| Logistic Regression | 0.765 | 0.768 | 0.765 |
| SGDClassifier | 0.759 | 0.764 | 0.758 |
| CatBoost | 0.719 | 0.745 | — |
| Decision Tree | 0.622 | 0.605 | — |

Test metrics for the two selected models:

| Metric | Logistic Regression | SGDClassifier |
|---|---:|---:|
| F1-score | 0.765 | 0.758 |
| ROC-AUC | 0.964 | 0.969 |
| Precision | 0.734 | 0.690 |
| Recall | 0.798 | 0.839 |
| Accuracy | 0.951 | 0.946 |

Logistic Regression achieved the highest test F1-score and precision. SGDClassifier produced higher recall and ROC-AUC, detecting a larger proportion of toxic comments.

Both models exceeded the required F1-score of 0.75.

## Limitations

- The dataset is strongly imbalanced.
- The split was performed without explicit stratification.
- TF-IDF was fitted before cross-validation instead of inside a pipeline.
- `pymystem3` is primarily intended for Russian-language text and provides limited value for English comments.
- Threshold optimization and transformer-based models were not evaluated.

Possible improvements include stratified splitting, a complete scikit-learn pipeline, threshold tuning and comparison with pretrained transformer models.

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- NLTK
- TF-IDF
- CatBoost
- Matplotlib

## Repository Contents

- [`Classification_model_NLP_BERT.ipynb`](./Classification_model_NLP_BERT.ipynb) — text preprocessing, model training and evaluation;
- `README.md` — project description and results.
