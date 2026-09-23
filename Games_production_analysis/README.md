# Video Game Market Analysis

An exploratory data analysis project investigating the factors associated with commercially successful video games.

The analysis covers platform life cycles, regional preferences, genres, critic and user scores, and ESRB ratings.

## Objective

Identify patterns that can help plan video game marketing campaigns and select promising platforms and genres.

The project examines:

- changes in game releases and sales over time;
- platform popularity and life cycles;
- relationships between reviews and sales;
- regional differences in customer preferences;
- differences in average user ratings.

## Dataset

The original dataset contains 16,715 video game records with:

- game title;
- platform;
- release year;
- genre;
- regional sales in North America, Europe, Japan and other markets;
- critic and user scores;
- ESRB age rating.

Records without a release year were removed, leaving 16,446 observations. Missing review scores were retained rather than replaced with artificial values.

The original dataset is not included in this repository.

## Approach

The project includes:

- data cleaning and type conversion;
- calculation of worldwide sales;
- analysis of platform life cycles;
- selection of 2013–2016 as the relevant market period;
- platform and genre comparisons;
- regional customer profiles;
- correlation analysis;
- Welch's independent two-sample t-tests.

## Results

### Platforms

The leading platforms by worldwide sales during 2013–2016 were:

| Platform | Total sales |
|---|---:|
| PS4 | 314.14 |
| PS3 | 181.43 |
| Xbox One | 159.32 |
| Nintendo 3DS | 143.25 |
| Xbox 360 | 136.80 |

The average observed platform life cycle was approximately eight years.

### Reviews and sales

For PS4 games:

| Relationship | Correlation |
|---|---:|
| User score and sales | -0.032 |
| Critic score and sales | 0.407 |

Across the selected period, user scores showed almost no linear relationship with sales, while critic scores had a moderate positive association.

Correlation does not imply that critic reviews directly cause higher sales.

### Genres

The genres with the highest total worldwide sales during 2013–2016 were:

| Genre | Total sales |
|---|---:|
| Action | 321.87 |
| Shooter | 232.98 |
| Sports | 150.65 |
| Role-Playing | 145.89 |

Action and Shooter dominated Western markets, while Role-Playing games were particularly important in Japan.

### Regional preferences

- PS4 and Xbox One were among the leading platforms in North America and Europe.
- Nintendo 3DS accounted for approximately 48% of Japanese platform sales.
- Action was the leading genre in North America and Europe.
- Role-Playing was the leading genre in Japan.
- Games rated `M` generated the largest sales in Western markets.
- Many Japanese releases had no ESRB rating recorded.

## Hypothesis Testing

| Hypothesis | p-value | Result |
|---|---:|---|
| Xbox One and PC have equal average user ratings | 0.1476 | Null hypothesis not rejected |
| Action and Sports have equal average user ratings | 1.45 × 10⁻²⁰ | Null hypothesis rejected |

The data did not provide sufficient evidence of different average user ratings between Xbox One and PC. A statistically significant difference was found between Action and Sports ratings.

## Limitations

- Review scores and ESRB ratings contain substantial missing data.
- Sales aggregates may favor genres with a larger number of releases.
- The 2016 data may be incomplete.
- Correlation analysis does not establish causality.
- Historical market patterns may not represent the current gaming market.
- This is an analytical study, not a predictive machine learning model.

## Technologies

- Python
- pandas
- NumPy
- SciPy
- Matplotlib
- Seaborn
- Plotly
- Jupyter Notebook

## Repository Contents

- [`Games_production_analysis.ipynb`](./Games_production_analysis.ipynb) — data preparation, exploratory analysis and hypothesis testing;
- `README.md` — project description and results.
