# Borrower Reliability Analysis

## Project Overview

This project investigates how selected borrower characteristics are associated with the probability of overdue loan repayment.

The analysis can support a bank's credit-scoring process by identifying borrower groups with historically different default rates.

## Objective

Determine whether loan repayment is associated with:

- the presence of children;
- marital status;
- income level;
- loan purpose.

This is an exploratory data analysis project. It identifies statistical associations in historical data rather than builds a predictive model.

## Dataset

The dataset contains **21,525 borrower records** and the following variables:

| Column | Description |
|---|---|
| `children` | Number of children |
| `days_employed` | Total employment duration in days |
| `dob_years` | Borrower's age |
| `education` | Education level |
| `education_id` | Education category identifier |
| `family_status` | Marital status |
| `family_status_id` | Marital-status identifier |
| `gender` | Gender |
| `income_type` | Employment type |
| `debt` | Whether the borrower previously had overdue debt |
| `total_income` | Monthly income |
| `purpose` | Purpose of the loan |

The original dataset is not included in this repository.

## Data Preprocessing

The following data-quality issues were identified and processed:

- **2,174 missing values** in `days_employed` and `total_income`;
- zero values in the age column;
- anomalous values of `-1` and `20` in the number of children;
- inconsistent letter case in education categories;
- incorrect data types;
- **71 duplicate records**;
- multiple textual descriptions of similar loan purposes.

Missing employment duration and income values were replaced with median values calculated for the corresponding age groups.

Zero age values were replaced using median borrower ages for the corresponding employment categories. The resulting borrower ages ranged from 19 to 75 years.

Education values were converted to lowercase, data types were corrected and duplicate records were removed.

## Feature Engineering

Borrowers were grouped by:

- age;
- income quartile;
- presence or absence of children;
- marital status;
- loan purpose.

Loan-purpose descriptions were processed using lemmatization and combined into five main categories:

- automobile;
- education;
- wedding;
- investment;
- real estate.

## Results

### Presence of Children

| Borrower Group | Overdue Debt Rate |
|---|---:|
| No children | 7.54% |
| Has children | 9.21% |

Borrowers with children had a higher historical overdue-debt rate in this dataset.

### Marital Status

| Marital Status | Overdue Debt Rate |
|---|---:|
| Unmarried | 9.75% |
| Civil partnership | 9.35% |
| Married | 7.55% |
| Divorced | 7.11% |
| Widowed | 6.57% |

Unmarried borrowers and borrowers in civil partnerships had the highest overdue-debt rates. Widowed and divorced borrowers had the lowest rates.

### Income Level

Income categories were constructed using the dataset quartiles.

| Income Category | Approximate Range | Overdue Debt Rate |
|---|---|---:|
| 1 | Up to 107,624 | 7.96% |
| 2 | 107,624–145,919 | 8.64% |
| 3 | 145,919–195,814 | 8.72% |
| 4 | Above 195,814 | 7.14% |

The highest-income category had the lowest overdue-debt rate. However, the relationship between income and repayment was not strictly monotonic across all four groups.

### Loan Purpose

| Loan Purpose | Overdue Debt Rate |
|---|---:|
| Automobile | 9.36% |
| Education | 9.22% |
| Wedding | 8.00% |
| Investment | 7.75% |
| Real estate | 7.08% |

Automobile and education loans were associated with the highest overdue-debt rates. Real-estate loans had the lowest rate.

## Conclusion

The analysis found that repayment behaviour differs across borrower groups.

In this dataset, a comparatively reliable borrower profile was associated with:

- no children;
- being or having previously been married;
- belonging to the highest-income category;
- borrowing for real estate.

The least favourable historical repayment rates were observed among borrowers taking loans for automobiles or education.

These results describe associations and should not be interpreted as proof that a particular characteristic causes loan default.

## Limitations

- The analysis uses aggregated groups and does not control for interactions between borrower characteristics.
- Unequal group sizes may affect the stability of the calculated rates.
- No statistical significance or confidence intervals were calculated.
- Income categories depend on the quartiles of this particular dataset.
- Missing values were imputed and anomalous values were corrected using assumptions.
- Sensitive personal characteristics should not be used in lending decisions without legal, ethical and fairness review.
- A production credit-risk system would require multivariate modelling, validation on new data and fairness monitoring.

## Technologies

- Python
- pandas
- pymystem3
- Jupyter Notebook

## Repository Contents

- `Research_of_borrowers_reliability.ipynb` — data preprocessing, borrower categorisation and repayment analysis
- `README.md` — project description and principal results
