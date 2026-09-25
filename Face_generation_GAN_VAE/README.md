# Face Generation with DCGAN and VAE

Generation and reconstruction of human face images using two generative deep learning approaches:

- Deep Convolutional Generative Adversarial Network (DCGAN);
- Variational Autoencoder (VAE).

The models are implemented with TensorFlow and Keras and trained on the deep-funneled version of the Labeled Faces in the Wild dataset.

## Objective

The project explores two different approaches to generative modeling:

1. Train a DCGAN to generate synthetic face images from random latent vectors.
2. Train a VAE to reconstruct existing faces and generate new samples from a continuous latent space.
3. Compare the visual properties and training behavior of both approaches.

## Dataset

The project uses the
[Labeled Faces in the Wild](http://vis-www.cs.umass.edu/lfw/)
dataset in its deep-funneled form.

A downloadable version containing the images and metadata is available on
[Kaggle](https://www.kaggle.com/datasets/jessicali9530/lfw-dataset).

Dataset characteristics:

```text
Images:      13,233
Identities:  5,749
Resolution:  250 × 250 pixels
Format:      JPEG
```

The dataset is not stored in this repository.

After downloading and extracting it, the expected directory structure is:

```text
Face_generation_GAN_VAE/
├── face_generation_gan_vae.ipynb
└── lfw-deepfunneled/
    └── lfw-deepfunneled/
        ├── Aaron_Eckhart/
        ├── Aaron_Guiel/
        ├── Aaron_Patterson/
        └── ...
```

## Data Preparation

Every image is:

1. loaded in RGB format;
2. resized from `250 × 250` to `64 × 64`;
3. converted to `float32`;
4. normalized from `[0, 255]` to `[-1, 1]`.

The data is randomly divided into:

| Subset | Images | Share |
|---|---:|---:|
| Training | 10,586 | 80% |
| Validation | 2,647 | 20% |

Only the training subset is used to update model weights. Validation images are used for visual comparison and VAE reconstruction analysis.

## DCGAN

The DCGAN consists of a generator and a discriminator trained adversarially.

### Generator

The generator converts a 100-dimensional random latent vector into a `64 × 64 × 3` RGB image.

```text
Latent vector: 100
    ↓
Dense and reshape: 4 × 4 × 512
    ↓
Conv2DTranspose: 8 × 8 × 256
    ↓
Conv2DTranspose: 16 × 16 × 128
    ↓
Conv2DTranspose: 32 × 32 × 64
    ↓
Conv2DTranspose: 64 × 64 × 3
    ↓
Tanh output
```

Batch normalization and LeakyReLU are used between transposed convolution layers.

### Discriminator

The discriminator classifies images as real or generated.

```text
Image: 64 × 64 × 3
    ↓
Conv2D: 32 × 32 × 64
    ↓
Conv2D: 16 × 16 × 128
    ↓
Conv2D: 8 × 8 × 256
    ↓
Flatten
    ↓
Real/fake logit
```

LeakyReLU and dropout are used after convolution layers.

### GAN Training

```text
Latent dimension:       100
Batch size:             64
Epochs:                 10
Generator optimizer:    Adam
Discriminator optimizer: Adam
Learning rate:          0.0002
Adam beta_1:            0.5
Loss:                   Binary cross-entropy
```

A fixed set of latent vectors is used to visualize the generator's progress after every epoch.

## Variational Autoencoder

The VAE consists of:

- convolutional encoder;
- sampling layer;
- 128-dimensional latent space;
- convolutional decoder.

### Encoder

```text
Image: 64 × 64 × 3
    ↓
Conv2D: 32 filters
    ↓
Conv2D: 64 filters
    ↓
Conv2D: 128 filters
    ↓
Conv2D: 256 filters
    ↓
Dense: 256
    ↓
z_mean and z_log_var: 128
```

The sampling layer uses the reparameterization trick:

```text
z = z_mean + exp(0.5 × z_log_var) × epsilon
```

### Decoder

```text
Latent vector: 128
    ↓
Dense and reshape: 4 × 4 × 256
    ↓
Conv2DTranspose: 8 × 8 × 128
    ↓
Conv2DTranspose: 16 × 16 × 64
    ↓
Conv2DTranspose: 32 × 32 × 32
    ↓
Conv2DTranspose: 64 × 64 × 16
    ↓
Conv2D: 64 × 64 × 3
    ↓
Tanh output
```

### VAE Loss

The objective combines:

- reconstruction loss;
- Kullback-Leibler divergence.

```text
Total loss = Reconstruction loss + KL loss
```

Training configuration:

```text
Latent dimension: 128
Batch size:       64
Epochs:           5
Optimizer:        Adam
Learning rate:    0.0005
```

## Results

### DCGAN

After 10 epochs, the generator learned several general characteristics of the dataset:

- centered face-like structures;
- approximate head contours;
- dark backgrounds;
- rough color and lighting patterns.

The generated images remain blurry and relatively similar to each other. This suggests insufficient training and partial mode collapse.

Final recorded losses:

| Metric | Value |
|---|---:|
| Generator loss | 1.2738 |
| Discriminator loss | 0.9408 |

### VAE

The VAE reconstruction loss decreased substantially during five epochs:

| Epoch | Reconstruction Loss | KL Loss | Total Loss |
|---:|---:|---:|---:|
| 1 | 2789.06 | 57.19 | 2846.25 |
| 2 | 1542.78 | 113.24 | 1656.02 |
| 3 | 1264.05 | 130.03 | 1394.08 |
| 4 | 1144.86 | 137.26 | 1282.12 |
| 5 | 1067.97 | 141.06 | 1209.03 |

The reconstructions preserve the approximate position and general structure of a face but remain strongly smoothed.

## Model Comparison

| Property | DCGAN | VAE |
|---|---|---|
| Training behavior | Less stable | More stable |
| Image contrast | Higher | Lower |
| Image smoothness | Lower | Higher |
| Sample diversity | Limited in this experiment | More consistent |
| Reconstruction capability | No | Yes |
| Main issue | Partial mode collapse | Blurry outputs |

DCGAN creates more contrasted samples but is sensitive to the balance between the generator and discriminator.

VAE trains more predictably and provides a structured latent space, but its pixel-based reconstruction loss produces smoother images.

The losses of DCGAN and VAE are defined differently and should not be compared directly.

## Technologies

- Python
- TensorFlow
- Keras
- NumPy
- pandas
- Pillow
- Matplotlib
- Jupyter Notebook

## Notebook

[Open the project notebook](./face_generation_gan_vae.ipynb)

The notebook contains intermediate and final generated images, so its file size is larger than that of the other projects in this repository.

## Running the Project

Install the required libraries:

```bash
pip install tensorflow numpy pandas pillow matplotlib jupyter
```

Place the extracted dataset in the project directory and start Jupyter:

```bash
jupyter notebook
```

Open `face_generation_gan_vae.ipynb` and run the cells in order.

A CUDA-compatible GPU is recommended, although the models can also be trained on a CPU.

## Limitations

- Images are reduced to `64 × 64` pixels.
- DCGAN is trained for only 10 epochs.
- VAE is trained for only 5 epochs.
- Generated images are evaluated visually without FID or KID.
- The DCGAN shows signs of partial mode collapse.
- VAE reconstructions are noticeably blurred.
- Model weights are not stored in the repository.
- The dataset contains real people and may reflect demographic and collection biases.

## Possible Improvements

- train both models for more epochs;
- use data augmentation;
- tune learning rates and batch sizes;
- increase image resolution;
- add checkpointing and early stopping;
- evaluate generation quality with FID or KID;
- use WGAN-GP for more stable adversarial training;
- compare results with a convolutional beta-VAE.

## Kaggle Notebook

[View the executed notebook on Kaggle](https://www.kaggle.com/code/khripushin/face-generation-with-dcgan-and-vae-on-lfw)
