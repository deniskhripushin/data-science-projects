# Customer Age Estimation

A computer vision project for estimating a person's age from a facial photograph.

The model uses a pretrained ResNet50 convolutional neural network and transfer learning to solve an image regression task.

## Objective

Build a neural network that predicts customer age from a photograph.

Model quality is evaluated using Mean Absolute Error (MAE). The target is to achieve an MAE below 7 years on the validation set.

## Dataset

The dataset contains 7,591 labeled facial images.

Each record includes:

| Field | Description |
|---|---|
| `file_name` | Image filename |
| `real_age` | Actual age of the person |

The images vary in lighting, quality, background and head angle. The age distribution is also imbalanced: a large proportion of the photographs represents people between 20 and 27 years old.

The original images are not included in this repository.

## Exploratory Analysis

The analysis identified several dataset characteristics:

- most photographs represent people aged 20–27;
- some ages appear more frequently around round values such as 25, 35, 50 and 60;
- image lighting and quality vary considerably;
- some faces are rotated or surrounded by dark borders;
- older age groups contain fewer observations.

These factors may reduce prediction accuracy for underrepresented age groups.

## Approach

The images were:

- resized to `224 × 224` pixels;
- normalized to the `[0, 1]` range;
- divided into training and validation subsets using a 75/25 split.

Dataset sizes:

| Subset | Images |
|---|---:|
| Training | 5,694 |
| Validation | 1,897 |

The model architecture consists of:

- ResNet50 pretrained on ImageNet;
- Global Average Pooling;
- a dense layer with 16 neurons and ReLU activation;
- a single-neuron regression output.

Training configuration:

| Parameter | Value |
|---|---|
| Optimizer | Adam |
| Learning rate | 0.0001 |
| Loss function | Mean Squared Error |
| Evaluation metric | MAE |
| Batch size | 16 |
| Epochs | 20 |

## Results

The model achieved:

| Metric | Result |
|---|---:|
| Best validation MAE | 5.96 years |
| Final validation MAE | 6.01 years |
| Final training MAE | 2.00 years |

The target requirement of MAE below 7 was achieved. On average, the final model's prediction differs from the actual age by approximately six years.

## Limitations

- The dataset has an uneven age distribution.
- Performance was evaluated on a validation split rather than a separate test set.
- The difference between training and validation MAE indicates some overfitting.
- No image augmentation beyond normalization was applied.
- Model quality may vary across age groups and image conditions.

Possible improvements include data augmentation, early stopping, learning-rate scheduling and age-group-specific evaluation.

## Technologies

- Python
- TensorFlow
- Keras
- ResNet50
- pandas
- NumPy
- Matplotlib
- Seaborn

## Repository Contents

- [`Customers_age_definition.ipynb`](./Customers_age_definition.ipynb) — exploratory analysis, model definition, training and evaluation;
- `README.md` — project description and results.
