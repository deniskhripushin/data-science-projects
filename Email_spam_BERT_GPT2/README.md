# Email Spam Classification with BERT and GPT-2

Binary classification of email messages as spam or ham using pretrained transformer embeddings and a PyTorch neural network.

The project compares representations obtained from:

- BERT (`bert-base-uncased`)
- GPT-2 (`gpt2`)

The transformer models are used as feature extractors. A separate feedforward neural network is trained on top of the resulting embeddings.

## Objective

Predict whether an email message belongs to the spam or ham class:

- `0` — ham
- `1` — spam

Because the classes are imbalanced, F1 score is used alongside accuracy to evaluate classification quality.

## Dataset

The experiment uses the first 1,000 records from `email_text.csv`.

Expected columns:

| Column | Description |
|---|---|
| `text` | Email message text |
| `label` | Target class: `0` for ham and `1` for spam |

The dataset is not stored in this repository.

Place `email_text.csv` in the project directory before running the notebook.

## Text Preprocessing

The preprocessing pipeline includes:

- expanding English contractions;
- converting text to lowercase;
- removing URLs;
- removing punctuation, numbers and special characters;
- removing extra whitespace;
- removing English stop words;
- WordNet lemmatization.

## Embedding Extraction

The cleaned texts are processed separately by BERT and GPT-2.

For both models:

- texts are truncated to 128 tokens;
- attention masks are used during pooling;
- token representations are combined with mean pooling;
- transformer weights are not fine-tuned;
- embeddings are generated in evaluation mode without gradient calculation.

## Classifier

The same PyTorch classifier is trained for both embedding types:

```text
Input embedding
    ↓
Linear layer: 256 units
    ↓
ReLU
    ↓
Dropout: 0.3
    ↓
Linear layer: 64 units
    ↓
ReLU
    ↓
Dropout: 0.2
    ↓
Binary output
```

Training configuration:

- stratified 80/20 train/test split;
- binary cross-entropy with logits;
- Adam optimizer;
- learning rate: `0.001`;
- 20 training epochs;
- random seed: `42`.

## Results

Metrics from the final training epoch:

| Model | Accuracy | F1 Score | Test Loss |
|---|---:|---:|---:|
| BERT | 0.9750 | 0.9645 | 0.1901 |
| GPT-2 | 0.9700 | 0.9571 | 0.0767 |

Both approaches achieved strong classification quality. BERT produced the highest final accuracy and F1 score, while GPT-2 produced the lower final test loss.

## Technologies

- Python
- PyTorch
- Hugging Face Transformers
- BERT
- GPT-2
- pandas and NumPy
- scikit-learn
- NLTK
- Matplotlib and Seaborn

## Notebook

[Open the project notebook](./email_spam_bert_gpt2.ipynb)

## Running the Project

Install the required libraries:

```bash
pip install torch transformers pandas numpy scikit-learn nltk contractions matplotlib seaborn
```

Place the dataset in the project directory:

```text
Email_spam_BERT_GPT2/
├── email_spam_bert_gpt2.ipynb
└── email_text.csv
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open `email_spam_bert_gpt2.ipynb` and run the cells in order.

The first run requires an internet connection to download the pretrained BERT and GPT-2 models and the required NLTK resources.

## Limitations

- Only the first 1,000 dataset records are used.
- Results are based on a single train/test split.
- BERT and GPT-2 are used only as frozen feature extractors.
- The transformer models are not fine-tuned for spam classification.
- Additional validation on the complete dataset is needed before production use.
