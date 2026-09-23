# Movie Genre Classification

Multiclass classification of movie genres from titles and plot descriptions using LSTM and DistilBERT.

The project compares a recurrent neural network trained from scratch with a pretrained transformer fine-tuned for 27-class text classification.

## Objective

Predict the genre of a movie using:

- movie title;
- plot description.

The target contains 27 genres:

```text
action, adult, adventure, animation, biography, comedy, crime,
documentary, drama, family, fantasy, game-show, history, horror,
music, musical, mystery, news, reality-tv, romance, sci-fi, short,
sport, talk-show, thriller, war and western
```

## Dataset

The project is based on the Kaggle
[Genre Classification Dataset IMDb](https://www.kaggle.com/datasets/hijest/genre-classification-dataset-imdb).

Dataset size:

```text
Training records: 54,214
Unlabeled records: 54,200
Number of genres: 27
```

Expected training columns:

| Column | Description |
|---|---|
| `name` | Movie title and release year |
| `genre` | Target movie genre |
| `text` | Plot description |

Expected test columns:

| Column | Description |
|---|---|
| `name` | Movie title and release year |
| `text` | Plot description |

The dataset is not stored in this repository.

Place `train.csv` and `test.csv` in the project directory before running the notebook.

### Converting the Original Kaggle Files

The original dataset may be distributed as `train_data.txt` and `test_data.txt`. They can be converted to the CSV format expected by the notebook:

```python
import pandas as pd

train = pd.read_csv(
    'train_data.txt',
    sep=r' ::: ',
    engine='python',
    names=['id', 'name', 'genre', 'text']
)

test = pd.read_csv(
    'test_data.txt',
    sep=r' ::: ',
    engine='python',
    names=['id', 'name', 'text']
)

train[['name', 'genre', 'text']].to_csv('train.csv', index=False)
test[['name', 'text']].to_csv('test.csv', index=False)
```

## Data Preparation

The movie title and plot description are combined into one feature:

```text
full_text = name + text
```

Genre names are converted into numerical labels using `LabelEncoder`.

Because the public `test.csv` does not contain genres, the labeled training data is divided into:

```text
Training:   70%
Validation: 15%
Test:       15%
```

Stratified splitting is used to preserve genre proportions.

Final split sizes:

| Subset | Records |
|---|---:|
| Training | 37,947 |
| Validation | 8,134 |
| Test | 8,133 |

## Class Imbalance

The dataset is strongly imbalanced.

The most frequent genres are:

- drama;
- documentary;
- comedy;
- short.

Rare genres such as war, news, history, biography and game-show contain substantially fewer examples.

For this reason, the project reports both weighted and macro F1 scores in addition to accuracy.

## LSTM Model

The baseline model is trained from scratch using PyTorch.

### Text Processing

- conversion to lowercase;
- removal of punctuation and special characters;
- whitespace tokenization;
- vocabulary limited to 30,000 tokens;
- `<PAD>` and `<UNK>` tokens;
- sequence length limited to 128 tokens.

### Architecture

```text
Token indices
    ↓
Embedding: 30,000 × 128
    ↓
LSTM: hidden size 128
    ↓
Dropout: 0.3
    ↓
Linear layer: 27 classes
```

Training configuration:

```text
Batch size:    64
Optimizer:     Adam
Learning rate: 0.001
Loss function: CrossEntropyLoss
Epochs:        5
```

Additional experiments compare LSTM hidden dimensions of 64, 128 and 256.

## DistilBERT Model

The transformer model uses:

```text
distilbert-base-uncased
```

A new classification head is trained for 27 genres while the pretrained DistilBERT weights are fine-tuned on the movie descriptions.

Configuration:

```text
Maximum sequence length: 128 tokens
Batch size:              16
Optimizer:               AdamW
Learning rate:           0.00002
Epochs:                  2
```

The model with the highest validation weighted F1 score is used for final test evaluation.

## Evaluation

The models are evaluated using:

- accuracy;
- weighted precision;
- weighted recall;
- weighted F1;
- macro F1;
- per-class classification report;
- confusion matrix.

Weighted F1 reflects overall performance while accounting for class frequency. Macro F1 gives equal importance to every genre and therefore highlights weak performance on rare classes.

## Results

| Model | Accuracy | Weighted Precision | Weighted F1 | Macro F1 |
|---|---:|---:|---:|---:|
| LSTM | 0.4527 | 0.3087 | 0.3613 | 0.0612 |
| Tuned LSTM, hidden size 128 | 0.4585 | 0.3313 | 0.3664 | 0.0619 |
| **DistilBERT** | **0.6712** | **0.6546** | **0.6571** | **0.4475** |

DistilBERT substantially outperformed the LSTM baseline across all reported metrics.

It achieved strong results for several frequent and distinctive genres, including:

- documentary;
- drama;
- comedy;
- horror;
- music;
- sport;
- game-show;
- western.

Rare or semantically similar genres remained more difficult to distinguish.

## Conclusion

The LSTM model mostly learned to predict the most frequent classes and performed poorly on rare genres.

DistilBERT produced much stronger results because its pretrained language representations capture the context and meaning of movie descriptions more effectively than embeddings learned from scratch.

The remaining performance gap between weighted and macro F1 shows that class imbalance is still a significant problem.

## Technologies

- Python
- PyTorch
- Hugging Face Transformers
- DistilBERT
- pandas and NumPy
- scikit-learn
- Matplotlib and Seaborn
- Jupyter Notebook

## Notebook

[Open the project notebook](./movie_genre_classification.ipynb)

## Running the Project

Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn torch transformers tqdm jupyter
```

Place the data files in the project directory:

```text
Movie_genre_classification/
├── movie_genre_classification.ipynb
├── train.csv
└── test.csv
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open `movie_genre_classification.ipynb` and run the cells in order.

The first run requires an internet connection to download `distilbert-base-uncased`. A CUDA-compatible GPU is strongly recommended for fine-tuning DistilBERT.

## Limitations

- The dataset has a substantial class imbalance.
- Several genres have very few training examples.
- Movies can naturally belong to multiple genres, but the task assigns only one class.
- Texts are truncated to 128 tokens.
- DistilBERT is fine-tuned for only two epochs.
- Class weighting and resampling are not used.
- The external unlabeled test set is not used to generate a submission.
