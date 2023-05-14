# IMA207 - Hyspectral Imaging

3 dimensional data (x,y,wavelength) -> 2 dimensional data (x,y) + 1 dimensional data (wavelength)

## 1. Introduction

High spectral resolution but low spatial resolution (typically about 10-30 meters)
because of the trade-off between the spatial and spectral resolution. Some channels might be very noisy.

## 2. Data processing for Hyspectral Imaging

A lot of pre-processing:
- Data calibration
- Atmospheric correction: 
  - remove the effets (absorption, scattering) of atmosphere, among others
- Geometric correction
- Radiometric correction
- Spectral calibration
- (...)

Processing for data exploitation:
- Denoising: 
- Compression: PCA, ICA, NMF, ...
- Target detection
- Material classification: SVM, KNN, ...
- Super-resolution
- **Unmixing**

we'll focus on hyperspectral unmixing (HSU). This is due to the fact that HSU
relies on the low-rank structure of the hyperspectral image, which is also a cornerstone for other data processing (super-resolution, compression...)

## 3. Hyperspectral unmixing (HSU)

We'll have pixels that are mixtures of several materials. We want to find the materials and their proportions. We'll assume that the materials are known, and we'll try to find the proportions.

### 3.1. Linear mixing model

$x_i = \sum^{n}_{k=1}a^*_k s_{ki} + n_i$

where $x_i$ is the $i^{th}$ pixel, $a_{k}$ is the $k^{th}$ material, $s^*_{ki}$ is the proportion of the $k^{th}$ material in the $i^{th}$ pixel, and $n_i$ is the noise.

(SLIDE 7) 
We can write this in matrix form:
$X = A^* S^* + N$

where $X$ is the hyperspectral image, $A^*$ is the matrix of the materials, $S^*$ is the matrix of the proportions, and $N$ is the noise.

our goal is to find $A^*$ and $S^*$ from the hyperspectral image $X$.
---

Blind Source Separation (BSS) is the problem of finding the sources $s_i$ from the mixtures $x_i$. This is a well-known problem in signal processing. We'll use the same techniques for HSU.

There are a lot of applications for BSS:
- Speech separation
- Image separation
- ...

=> We'll use the same techniques for HSU.

HSU/BSS vs dimensionality reduction:
- HSU/BSS: we want to find the sources
- Dimensionality reduction: we want to find the low-dimensional representation of the data
  
In IMA205: The general goal of unsupervised
machine learning is to find a low-dimensional representation of the data. (describe the data with a smaller set of latent variables)

In IMA207: we want to find the sources (the materials) and their proportions. (Recover the (small dimensional) set of latent variables that generated the data
set)

Let $A^*$,$S^*$ be such that:
$X = A^* S^*$
$X = A^* PP^{-1} S^*$ with P any invertible matrix
$X = A S$
where $A = A^* P$ and $S = P^{-1} S^*$

Thus, there is a infinite number of possible solutions for $A$ and $S$. which do not correspond to the same physical reality.

We need to add constraints to the problem to find a unique solution.

### Introducing additional priors (constraints)

Three main families of priors in BSS :
- Assume the independence of the sources $S$ (ICA)
- Assume the sparsity of $S$ (SBSS - sparse BSS)  
- Assume the non-negativity of $A$ and $S$ (NMF - non-negative matrix factorization)

#### 3.1.1. Independent Component Analysis (ICA)

We assume that the sources are independent. This is a strong assumption, but their mixtures are not.

two types of methods to solve the problem:
- minimization of the mutual information between the sources
- maximization of the non-Gaussianity of the sources

Pros: 
Darmois-Skitov theorem: the sum of independent random variables is less Gaussian than the variables themselves provide that:
- All the sources are statistically independent
- There is at most one Gaussian source
- The mixing matrix is invertible (square and full rank)

Cons:
- The Darmois-Skitov theorem result only holds in the absence of noise
- The statistical independence of the sources is a strong assumption because the sources are not necessarily independent in the mixtures since they are mixed by the same physical process

#### 3.1.2. Sparse Blind Source Separation (SBSS)

We assume that the sources are sparse. This is a weaker assumption than independence. 

Exact sparsity: A signal is said to be k-sparse if only k << t of its elements are non-zero.
$||s||_0 = k << t$

Exemple: Emission spectrum of hydrogem is a real-life exactly sparse signal

Most real-life signals are not exactly sparse, but approximately sparse.

Only a small number k of the signal samples have a large amplitude.

Approximate sparsity: A signal is said to be approximately k-sparse if only k << t of its elements are significantly different from zero.

#### Approximately sparse signals in transformed domains

Most signals are not (exactly of approximately) sparse in the direct domain
However, they admit an approximately sparse representation in a transformed domain $\Phi$.
In practice, it is often better to retain the spatial information (e.g. by using a multi-scale transform - wavelet)

Its also possible to learn $\Phi$. This is called dictionary learning.

#### Interpretation of sparse BSS: intuition

Now that we introduced sparsity, how can we use it for BSS?

We'll use the fact that the sources are sparse in the transformed domain $\Phi$.
Mixing reduces the sparsity. To recover the sources, we must find the sparsest signals.

*Geometrical view*: Due to sparsity, the scatter plot of the sources has a star shape. The mixing matrix is a rotation and a scaling. The scatter plot of the mixtures is an ellipse. Recovering the sources amounts to back-project the observations on the canonical axes Sparse Blind Source Separation: interpretation.

*Statistical view*: rewrite sparse BSS as a Maximum A Posteriori (MAP) estimation.

$argmax_{A,S} P(A,S|X)$

By bayes' rule
$P(A,S|X) \propto P(X|A,S)P(A,S)$

since A and S are independent:
$P(A,S|X)  \propto P(X|A,S)P(A)P(S)$

$P(X|A,S)$ is the likelihood of the data given the model. It is the probability of observing the data X given the model parameters A and S. 

using -log:

$argmin_{A,S} -log(P(X|A,S)) -log(P(A)) -log(P(S))$

Assuming:
- A Gaussian noise: P(X|A,S) is a Gaussian distribution
- An exponential distribution for S
- A uniform distribution for A
  
we obtain:

$argmin_{A,S} \frac{1}{2\sigma^2}||X-AS||^2_F + \beta ||S||_1$

$argmin_{A,S} \frac{1}{2}||X-AS||^2_F + \lambda ||S||_1$

where $\lambda$ is a regularization parameter.

The scaling indeterminacy: 
...

More generally, we need to modify the cost function and limit the energy of the columns of $A$ so that we do not obtain degenerated solutions

**Oblique constraint**
We require each col of A to have unit norm
We obtain the following cost-function





