# Book Recommendation with ALS

An offline evaluation of book recommendation approaches using the Goodbooks-10k dataset.

The project compares:

- random recommendations;
- popularity-based recommendations;
- collaborative filtering with Alternating Least Squares;
- hybrid recommendations using ALS candidate generation and Random Forest reranking.

## Objective

Build a recommendation system that generates relevant book recommendations for each user and evaluate its quality using `mAP@10`.

Ratings of 4 or 5 are treated as positive interactions. Ratings below 4 are treated as negative feedback.

## Dataset

The project uses the Goodbooks-10k dataset:

- 5,976,479 ratings;
- 53,424 users;
- 10,000 books;
- ratings from 1 to 5.

The dataset is not stored in this repository.

Download `ratings.zip` from the
[Goodbooks-10k releases page](https://github.com/zygmuntz/goodbooks-10k/releases),
extract `ratings.csv` and place it in this project directory.

Expected columns:

| Column | Description |
|---|---|
| `user_id` | User identifier |
| `book_id` | Book identifier |
| `rating` | Rating from 1 to 5 |

## Approach

### Train/Test Split

The interaction history of every user is divided into:

- first 70% — training data;
- last 30% — test data.

Only positive training interactions with ratings of 4 or 5 are included in the implicit user-item matrix.

### Baselines

Two baseline recommenders are evaluated:

1. **Random baseline** — recommends random unseen books.
2. **Popular baseline** — recommends the most popular unseen books.

### ALS

Collaborative filtering is implemented using implicit Alternating Least Squares.

Configuration:

```text
Factors:        64
Iterations:     20
Regularization: 0.01
Random seed:    42
```

Books already present in the user's training history are excluded from recommendations.

### Hybrid Model

The hybrid model uses two stages:

1. ALS generates 30 candidate books.
2. `RandomForestClassifier` reranks the candidates by the estimated probability of receiving a positive rating.

User features include:

- number of ratings;
- average rating;
- number of positive ratings;
- positive-rating rate.

Book features include:

- number of ratings;
- average rating;
- number of positive ratings;
- positive-rating rate.

## Evaluation

Recommendation quality is evaluated using Average Precision at 10 and mean Average Precision at 10.

The same sample of 500 users is used for every model.

## Results

| Model | mAP@10 |
|---|---:|
| Random baseline | 0.000860 |
| Popular baseline | 0.059875 |
| ALS | **0.138668** |
| Hybrid ALS + Random Forest | 0.084021 |

ALS achieved the best result and substantially outperformed both baselines.

The hybrid model performed better than the popularity baseline but worse than standalone ALS. In the current implementation, the Random Forest reranker weakens the ranking produced by ALS.

## Technologies

- Python
- pandas and NumPy
- SciPy sparse matrices
- implicit
- scikit-learn
- Jupyter Notebook

## Notebook

[Open the project notebook](./book_recommendation_als.ipynb)

## Running the Project

Install the required libraries:

```bash
pip install pandas numpy scipy scikit-learn implicit jupyter
```

Place `ratings.csv` in the project directory:

```text
Book_recommendation_ALS/
├── book_recommendation_als.ipynb
└── ratings.csv
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open `book_recommendation_als.ipynb` and run the cells in order.

The notebook generates the following intermediate files:

```text
user_features.csv
book_features.csv
```

These files do not need to be downloaded separately.

## Limitations

- Evaluation is performed on a sample of 500 users.
- The split relies on the order of records as a proxy for chronology.
- No explicit interaction timestamps are used.
- The hybrid reranker uses aggregated user and book statistics only.
- Hyperparameter optimization has not been performed.
- Results are based on offline evaluation and do not measure user satisfaction in a production environment.
