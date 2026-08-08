---
type: computational-pipeline
name: Linear Regression Computational Pipeline
algorithm:
  - "[[Linear Regression]]"
status: reviewed
tags:
  - computational-pipeline
  - linear
---

# Linear Regression Computational Pipeline

## 1. Mathematical Foundations

[[Vector Space]]
    ↓
[[Euclidean Norm]]
    ↓
[[Matrix Multiplication]]
    ↓
[[Matrix Rank]]
    ↓
[[Least Squares]]

Statistical interpretation additionally depends on:

[[Expected Value]]
    ↓
[[Variance]]
    ↓
Conditional mean model
```

## 2. Formal Model

$$
y=X\beta+\varepsilon
$$

## 3. Objective

$$
\hat{\beta}
=
\arg\min_{\beta}
\lVert y-X\beta\rVert_2^2
$$

Related note: [[Mean Squared Error]].

## 4. Solver Branch

```text
Least-squares objective
    ├── [[Normal Equations]]
    ├── [[QR Decomposition]]
    ├── [[Singular Value Decomposition]]
    └── sparse iterative least-squares solver
```

The branch chosen affects numerical stability, rank handling, runtime, and memory.

## 5. High-Level Implementations

```text
[[Linear Regression]]
    ├── [[scikit-learn - LinearRegression]]
    ├── [[statsmodels - OLS]]
    ├── [[NumPy - lstsq]]
    ├── [[SciPy - lstsq]]
    ├── [[PyTorch - Linear Regression]]
    └── [[TensorFlow - Linear Regression]]
```

## 6. scikit-learn Dense Execution Path

```text
sklearn.linear_model.LinearRegression.fit
        ↓
input validation
        ↓
preprocessing and optional centering
        ↓
optional sample-weight rescaling
        ↓
scipy.linalg.lstsq
        ↓
selected LAPACK least-squares driver
        ↓
BLAS / LAPACK implementation
        ↓
CPU
```

## 7. scikit-learn Sparse Execution Path

```text
LinearRegression.fit with sparse X
        ↓
intercept-aware linear operator
        ↓
LSQR
        ↓
sparse matrix-vector products
        ↓
CPU
```

The cost depends on iterations and nonzero entries rather than dense matrix dimensions alone.

## 8. PyTorch Training Path

A PyTorch linear layer plus MSE loss is usually trained iteratively rather than solved directly:

```text
torch.nn.Linear
        ↓
forward matrix multiply
        ↓
MSE loss
        ↓
autograd
        ↓
optimizer step
        ↓
ATen operators
        ↓
CPU kernels or CUDA libraries
        ↓
CPU or GPU
```

For $$T$$ full-batch gradient steps, a dominant dense cost is approximately:

$$
O(Tnp)
$$

This is not the same computational algorithm as a direct QR or SVD least-squares solve, even when both target the same unregularized objective.

## 9. TensorFlow Training Path

```text
tf.keras layer or tensor variables
        ↓
forward linear operation
        ↓
MSE loss
        ↓
GradientTape / graph differentiation
        ↓
optimizer
        ↓
TensorFlow runtime and device kernels
        ↓
CPU, GPU, or accelerator
```

## 10. Hardware Interpretation

### CPU

Dense least-squares routines generally use compiled numerical libraries and exploit:

- vector instructions
- cache-aware kernels
- multithreaded BLAS where configured
- optimized LAPACK routines

### GPU

GPU execution is most natural for large batched tensor operations or iterative training. Transferring a modest tabular dataset to a GPU can cost more than the compute saved.

## 11. Application Layer

The fitted coefficients are used for:

- continuous-outcome prediction
- baseline modelling
- effect estimation under statistical assumptions
- adjustment for covariates
- trend estimation
- feature-effect interpretation

## 12. Bidirectional Navigation

From theory to code:

[[Least Squares]]
    ↓
[[Linear Regression]]
    ↓
[[scikit-learn - LinearRegression]]
    ↓
[[SciPy - lstsq]]
    ↓
[[LAPACK]]
    ↓
[[CPU]]

From code to theory:

[[LinearRegression.fit]]
    ↑ implements
[[Linear Regression]]
    ↑ minimizes
[[Least Squares]]
    ↑ depends on
[[Euclidean Norm]]
