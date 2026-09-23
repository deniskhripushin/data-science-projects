# Telecom User Behavior and Tariff Revenue Analysis

## Project Overview

This project analyses the behaviour of customers using two prepaid mobile plans: **Smart** and **Ultra**.

The analysis compares customer activity, monthly charges and revenue to determine which tariff is more commercially valuable to the mobile operator.

## Objectives

The project addresses the following questions:

- How do customers of the Smart and Ultra plans use calls, messages and mobile internet?
- How much monthly revenue does each tariff generate?
- Is the difference in average revenue between the tariffs statistically significant?
- Does average revenue from customers in Moscow differ from revenue in other regions?

## Datasets

The analysis uses five related datasets.

| Dataset | Records | Description |
|---|---:|---|
| Calls | 202,607 | Call dates, durations and customer identifiers |
| Internet | 149,396 | Internet-session dates and traffic volume |
| Messages | 123,036 | Message dates and customer identifiers |
| Users | 500 | Customer information, location and tariff |
| Tariffs | 2 | Monthly fees, included allowances and overage prices |

The source datasets are not included in this repository.

## Tariff Conditions

| Parameter | Smart | Ultra |
|---|---:|---:|
| Monthly fee | 550 RUB | 1,950 RUB |
| Included minutes | 500 | 3,000 |
| Included messages | 50 | 1,000 |
| Included internet traffic | 15 GB | 30 GB |
| Additional minute | 3 RUB | 1 RUB |
| Additional message | 3 RUB | 1 RUB |
| Additional internet traffic | 200 RUB/GB | 150 RUB/GB |

Of the 500 customers in the sample:

- 351 used the Smart plan;
- 149 used the Ultra plan.

## Data Preprocessing

The following preprocessing steps were performed:

- date columns were converted to `datetime`;
- customer names and city names were converted to lowercase;
- an unnecessary internet-table index column was removed;
- call durations were rounded up according to the billing rules;
- internet usage was rounded for revenue calculation;
- month numbers were extracted from activity dates;
- missing monthly activity values were replaced with zeros;
- call, message and internet records were aggregated by customer and month;
- customer and tariff information was joined into a single analytical table.

Monthly revenue was calculated as the tariff subscription fee plus charges for usage exceeding the included allowances.

## Customer Behaviour

Average monthly usage calculated from customer-month observations:

| Metric | Smart | Ultra |
|---|---:|---:|
| Call duration | 429 minutes | 541 minutes |
| Messages | 33 | 49 |
| Internet traffic | 16.2 GB | 19.5 GB |

Ultra customers used more of every service on average. Their behaviour was also more variable, as indicated by the higher standard deviations.

### Usage Variability

| Metric | Smart Standard Deviation | Ultra Standard Deviation |
|---|---:|---:|
| Call duration | 195 minutes | 326 minutes |
| Messages | 28 | 48 |
| Internet traffic | 5.9 GB | 10.1 GB |

Smart customers frequently exceeded their included limits, especially for mobile internet. Ultra customers paid considerably less for additional usage because their tariff included much larger service packages.

## Revenue Analysis

The notebook's month-level summary produced the following approximate average monthly charges:

| Tariff | Average Monthly Revenue |
|---|---:|
| Smart | 1,146 RUB |
| Ultra | 2,039 RUB |

Although Smart customers spent less on average, they frequently generated additional revenue through overage charges. Ultra customers generated substantially higher average revenue primarily because of the higher monthly subscription fee.

In this project, these values represent **revenue**, not profit, because the operator's service costs are not provided.

## Hypothesis Testing

A significance level of **0.05** was used for the statistical tests.

### Revenue by Tariff

**Null hypothesis:** average monthly revenue from Smart and Ultra customers is equal.

**Alternative hypothesis:** average monthly revenue differs between the two tariffs.

The two-sample t-test produced:

- **p-value:** approximately `4.12 × 10⁻¹⁸⁰`

Because the p-value is far below 0.05, the null hypothesis was rejected. The observed difference in average revenue between the Smart and Ultra plans is statistically significant.

### Revenue by Region

**Null hypothesis:** average revenue from customers in Moscow is equal to average revenue from customers in other regions.

**Alternative hypothesis:** average revenue differs between Moscow and other regions.

The test produced:

- **p-value:** approximately `0.519`

The null hypothesis could not be rejected. The available data does not provide sufficient evidence that average revenue in Moscow differs from average revenue in other regions.

## Conclusion

Ultra customers use more minutes, messages and mobile internet and generate higher average monthly revenue than Smart customers.

From the operator's revenue perspective, **Ultra is the more valuable tariff per customer**. However, the total commercial contribution of each plan also depends on the number of subscribers, customer acquisition costs, service costs, retention and churn.

Smart is less expensive for customers on average, but its relatively small included packages result in frequent additional charges.

## Limitations

- The project analyses revenue rather than actual profit.
- The dataset covers only one calendar year.
- Customer acquisition, retention and service costs are unavailable.
- The samples for the two tariffs have different sizes.
- The standard t-test assumes conditions that should be checked explicitly, including variance behaviour and independence.
- Welch's t-test would be preferable when group variances differ.
- Failing to reject the regional null hypothesis does not prove that revenues are identical.
- Two users without recorded monthly activity were assigned zero-month observations and subscription fees; these rows should be excluded in a revised analysis.
- Internet billing should be verified against the precise operator rule for rounding monthly traffic to gigabytes.

## Technologies

- Python
- pandas
- NumPy
- SciPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## Repository Contents

- `User_behavior_analysis.ipynb` — data preprocessing, customer behaviour analysis, revenue calculation and hypothesis testing
- `README.md` — project description and principal results
