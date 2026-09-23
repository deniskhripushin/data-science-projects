# Flight Demand Analysis

An exploratory data analysis project examining airline flight activity across aircraft models and Russian cities.

The project also tests whether passenger demand for flights to Moscow differs between festival and non-festival weeks.

## Objective

The analysis addresses three questions:

- Which aircraft models operated the largest number of flights?
- Which cities received the highest average number of daily flights?
- Does average ticket demand differ between festival and non-festival weeks?

## Data

The project uses three datasets generated from SQL queries.

### Aircraft activity

Contains eight aircraft models:

| Feature | Description |
|---|---|
| `model` | Aircraft model |
| `flights_amount` | Number of flights performed in September 2018 |

### City activity

Contains data for 101 Russian cities:

| Feature | Description |
|---|---|
| `city` | Destination city |
| `average_flights` | Average number of arriving flights per day |

### Festival weeks

Contains ticket demand for ten weeks:

| Feature | Description |
|---|---|
| `week_number` | Calendar week number |
| `ticket_amount` | Number of tickets sold for flights to Moscow |
| `festival_week` | Festival week number |
| `festival_name` | Festival name |

The original datasets are not included in this repository.

## Approach

The project includes:

- data validation and descriptive statistics;
- ranking aircraft models by flight activity;
- ranking cities by average daily arrivals;
- visualization of flight activity;
- comparison of festival and non-festival weeks;
- independent two-sample t-test with a significance level of 0.05.

Statistical hypotheses:

- **H₀:** average ticket demand is the same during festival and non-festival weeks;
- **H₁:** average ticket demand differs between the two groups.

## Results

The three cities with the highest average number of arriving flights were:

| City | Average daily flights |
|---|---:|
| Moscow | 129.77 |
| Saint Petersburg | 31.16 |
| Novosibirsk | 17.32 |

Moscow was substantially ahead of every other destination in average daily arrivals.

The statistical comparison used three festival weeks and seven non-festival weeks.

| Test | Result |
|---|---:|
| Significance level | 0.05 |
| t-test p-value | 0.0969 |

Since the p-value is greater than 0.05, the null hypothesis was not rejected. The available data does not provide sufficient evidence of a statistically significant difference in ticket demand between festival and non-festival weeks.

This result does not prove that festivals have no effect on demand.

## Limitations

- The hypothesis test is based on only ten weekly observations.
- Only three observations correspond to festival weeks.
- The assumptions of the independent t-test were not fully verified.
- Weekly observations may contain time-related dependencies.
- The notebook's Mann–Whitney calculation compares the U statistic with the significance level; a corrected implementation should compare its p-value instead.
- The analysis identifies associations and does not establish causality.

## Technologies

- Python
- pandas
- NumPy
- SciPy
- Matplotlib
- Jupyter Notebook

## Repository Contents

- [`Flights_query_analysis.ipynb`](./Flights_query_analysis.ipynb) — exploratory analysis, visualization and hypothesis testing;
- `README.md` — project description and results.
