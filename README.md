# Learning Probability Density Functions Directly from Data

## Dataset Information

* **Dataset Name:** India Air Quality Data
* **Feature Selected:** NO₂ (Nitrogen Dioxide) concentration
* **Source:** Kaggle
* **Link:** [https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data](https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data)

For this project, only the **NO₂ column** is used. All missing or invalid entries are removed before performing any analysis or modeling.

---

## Project Overview

This project demonstrates how a probability density function (PDF) can be learned directly from data without assuming any predefined statistical distribution. Instead of fitting a normal, exponential, or any other known distribution, a **Generative Adversarial Network (GAN)** is trained to learn the underlying structure of transformed NO₂ concentration values.

---

## Methodology

### 1. Data Preparation

* The NO₂ values are extracted from the dataset.
* Missing values are removed to ensure clean input.
* The cleaned data is normalized to improve numerical stability during neural network training.

Normalization helps in faster convergence and stable GAN training.

---

### 2. Non-Linear Data Transformation

To make the distribution more complex and unknown, each data value ( x ) is transformed using:

[
z = x + a_r \sin(b_r x)
]

The parameters ( a_r ) and ( b_r ) are derived from the university roll number ( r ):

[
a_r = 0.5 \times (r \bmod 7)
]

[
b_r = 0.3 \times ((r \bmod 5) + 1)
]

This transformation introduces controlled non-linearity into the dataset. As a result, the new variable ( z ) follows a distribution that does not match standard parametric forms, making it suitable for learning via GAN.

---

### 3. GAN Model Design

A **Generative Adversarial Network (GAN)** is used to learn the transformed data distribution.

The GAN consists of two neural networks:

#### Generator

* Accepts random noise sampled from a standard normal distribution ( N(0,1) ).
* Produces synthetic samples intended to resemble the transformed NO₂ data.

#### Discriminator

* Receives both real transformed samples and generator-produced samples.
* Learns to distinguish between real and synthetic data.

Both networks are trained simultaneously in an adversarial setup. The generator improves by trying to fool the discriminator, while the discriminator improves by correctly identifying fake samples. No explicit probability distribution is assumed at any stage.

---

### 4. Estimating the Probability Density Function

After the GAN training process is complete:

1. A large number of synthetic samples are generated using the trained generator.
2. Kernel Density Estimation (KDE) is applied to these generated samples.
3. The estimated PDF is plotted.
4. The estimated density curve is compared with the histogram of the real transformed dataset.

This allows visual evaluation of how well the GAN has learned the distribution.

---

## Results and Observations

### Mode Representation

The generated samples successfully capture the major peaks (modes) of the transformed data distribution. The overlap between the histogram and the estimated density indicates that the GAN has learned the dominant patterns in the data.

### Training Behavior

Oscillations are observed in generator and discriminator losses during training, which is typical behavior in GAN-based models. Despite this, the training process remains stable and does not collapse or diverge.

### Distribution Quality

The probability density estimated from GAN-generated samples aligns closely with the actual transformed data distribution. This demonstrates that GANs can effectively learn complex, non-linear probability distributions directly from data without assuming any predefined parametric model.


