# Real Estate Market Analysis

An exploratory data analysis project examining residential property listings in Saint Petersburg and the surrounding region.

The project investigates apartment prices, listing duration, geographic patterns and the property characteristics most strongly associated with market value.

## Objective

Analyze historical real estate listings to:

- identify factors associated with apartment prices;
- calculate price per square meter;
- examine typical property sale times;
- detect unusual and anomalous listings;
- compare Saint Petersburg with surrounding localities;
- analyze apartments located near the city center.

This is an analytical project rather than a predictive machine learning model.

## Data

The original dataset contains 23,699 property listings and 22 features.

The variables include:

- listing price;
- total, living and kitchen area;
- number of rooms;
- ceiling height;
- floor and total number of floors;
- balcony availability;
- locality name;
- distance to the city center and airport;
- nearby parks and ponds;
- listing publication date;
- number of days the listing remained active.

The original dataset is not included in this repository.

## Data Preparation

The preprocessing stage includes:

- handling missing values;
- converting columns to appropriate data types;
- calculating price per square meter;
- extracting publication weekday, month and year;
- classifying apartments by floor type;
- calculating living-area and kitchen-area ratios;
- identifying unusually large, expensive or long-running listings;
- filtering selected outliers.

After basic preprocessing, 23,565 listings remained. The final filtered analytical dataset contains 12,574 listings.

## Approach

The analysis examines relationships between listing price and:

- total apartment area;
- number of rooms;
- floor;
- distance from the city center;
- listing duration;
- publication date;
- locality.

Apartments in Saint Petersburg were analyzed separately. Distance to the city center was converted from meters to kilometers.

A noticeable price change was observed around 8–9 kilometers from the city center. For a more focused central-area analysis, the notebook uses listings located within 5 kilometers.

## Results

### Factors associated with price

| Factor | Correlation with price |
|---|---:|
| Total area | 0.686 |
| Number of rooms | 0.413 |
| Floor | 0.194 |
| Distance from city center | -0.202 |
| Listing duration | 0.027 |

Total area has the strongest positive relationship with price.

The number of rooms also has a noticeable positive relationship with price, partly because it is associated with apartment size.

Distance from the city center has a negative relationship with price: listings farther from central Saint Petersburg tend to be less expensive.

Listing duration has almost no linear relationship with price in the filtered dataset.

### Price per square meter

Median price per square meter among localities with the largest number of listings:

| Locality | Listings | Median price per m² |
|---|---:|---:|
| Saint Petersburg | 7,674 | 100,337 RUB |
| Murino | 359 | 86,232 RUB |
| Shushary | 310 | 75,650 RUB |
| Vsevolozhsk | 253 | 65,833 RUB |
| Pargolovo | 229 | 90,837 RUB |

Saint Petersburg has the highest median price per square meter among the most frequently represented localities.

### Central Saint Petersburg

The central-area subset contains 494 listings located within 5 kilometers of the city center.

Typical characteristics:

| Characteristic | Median value |
|---|---:|
| Total area | 62.85 m² |
| Price | 7.1 million RUB |
| Number of rooms | 2 |
| Ceiling height | 2.65 m |
| Listing duration | 118 days |

Central apartments are generally larger and more expensive than listings in the wider dataset.

## Conclusion

The most important property characteristics associated with price are:

- total area;
- number of rooms;
- distance from the city center;
- floor;
- locality.

Total apartment area has the strongest observed relationship with price. Central Saint Petersburg listings command higher prices, while properties farther from the center tend to be less expensive.

Publication weekday, month and listing duration show much weaker relationships with market value.

## Limitations

- A substantial share of the original data contains missing values.
- Some missing values were replaced using global or locality-level averages.
- Missing listing duration was replaced with zero, which may mix active listings with genuinely short sales.
- Outlier filtering reduced the dataset from 23,565 to 12,574 observations and may have removed valid premium properties.
- Correlation does not establish a causal relationship.
- The project does not build or evaluate a predictive pricing model.
- Geographic analysis is based primarily on distance rather than detailed spatial coordinates.

Possible improvements include more conservative outlier treatment, separate handling of active listings, geospatial visualization and development of a validated price-prediction model.

## Technologies

- Python
- pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## Repository Contents

- [`Model_for_defenition_of_real_estate_price.ipynb`](./Model_for_defenition_of_real_estate_price.ipynb) — data preprocessing, exploratory analysis and visualization;
- `README.md` — project description and results.
