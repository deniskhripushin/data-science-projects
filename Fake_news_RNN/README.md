# Fake News Classification with RNNs

Binary classification of news records using recurrent neural networks implemented in PyTorch.

The project compares three neural network architectures:

- Simple RNN
- LSTM
- GRU

## Objective

Predict whether a news record is real or fake.

The target variable is `real`:

- `1` — real news
- `0` — fake news

After removing missing values, the dataset contains 22,866 records. The class distribution is imbalanced: approximately 76% real news and 24% fake news.

## Approach

1. Remove records containing missing values.
2. Combine the news title, URL and source domain into one text feature.
3. Convert text to lowercase and remove punctuation.
4. Split the data into training and validation sets using stratification.
5. Train Word2Vec embeddings on the training texts.
6. Convert texts into sequences of token indices.
7. Pad or truncate sequences to 100 tokens.
8. Train and compare RNN, LSTM and GRU classifiers.
9. Evaluate the models using Accuracy, F1 score and ROC-AUC.

## Model Architecture

All three models use:

- trainable Word2Vec embeddings;
- 100-dimensional word vectors;
- hidden state size of 128;
- dropout regularization;
- binary cross-entropy loss;
- Adam optimizer.

Packed sequences are used so the recurrent layers ignore padding tokens.

## Results

Metrics reported for the final training epoch:

| Model | Accuracy | F1 Score | ROC-AUC | Best observed ROC-AUC |
|---|---:|---:|---:|---:|
| RNN | 0.7787 | 0.8553 | 0.7758 | 0.9707 |
| LSTM | 0.9895 | 0.9931 | 0.9980 | 0.9983 |
| GRU | 0.9880 | 0.9921 | 0.9979 | 0.9981 |

LSTM and GRU substantially outperformed the basic RNN. LSTM produced the highest observed ROC-AUC.

## Technologies

- Python
- PyTorch
- Gensim and Word2Vec
- pandas and NumPy
- scikit-learn
- TensorFlow/Keras preprocessing utilities
- Matplotlib and Seaborn
- Yellowbrick

## Data

The dataset is not stored in this repository.

To run the notebook, place the following file in the project directory:

```text
FakeNewsNet.csv
```

Expected columns:

```text
title
news_url
source_domain
tweet_num
real
```

## Notebook

[Open the project notebook](./fake_news_rnn.ipynb)

## Running the Notebook

Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn yellowbrick torch gensim tensorflow
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Then open `fake_news_rnn.ipynb` and run the cells in order.

## Limitations

The model uses the news URL and source domain in addition to the title. Therefore, it may learn patterns associated with particular websites rather than only the linguistic properties of fake news.

The random train/test split may also place records from the same domains in both subsets. A stricter evaluation would split the data by source domain or publication date.

Saved model weights and the source dataset are excluded from the repository.
