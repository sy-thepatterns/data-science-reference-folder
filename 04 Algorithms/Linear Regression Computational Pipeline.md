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

```text
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

## Notation

| Symbol | Meaning |
|---|---|
| $n$ | Number of observed examples or rows. |
| $p$ | Number of input features or columns. |
| $x_i$ | Feature vector for example $i$. |
| $y_i$ | Observed target or label for example $i$. |
| $X$ | Design matrix whose row $i$ is $x_i^T$; usually $X\in\mathbb{R}^{n\times p}$. |
| $y$ | Vector of all observed targets. |
| $\theta$ | Generic collection of parameters learned by a model. |
| $\ell$ | Loss assigned to a prediction and its observed target. |
| $\beta_0$ | Intercept: the prediction when all represented features are zero. |
| $\beta$ | Vector of $p$ coefficients; $\beta_j$ controls feature $j$ while other represented features are held fixed. |
| $\hat{\beta}$ | Estimated coefficient vector; a hat marks a quantity learned from data. |
| $X\beta$ | Vector of linear predictions before adding a separate intercept. |
| $\varepsilon$ | Unobserved error: the part of $y$ not represented by the linear mean model. |
| $r=y-X\beta$ | Residual vector: observed values minus fitted values. |
| $\hat y$ | Vector of fitted or predicted responses. |
| $w_i$ | Optional nonnegative importance weight for example $i$. |
| $\mathbb{E}[\cdot]$ | Expected value under the stated probability model. |
| $\operatorname{Var}(\cdot)$ | Variance, measuring squared spread around an expectation. |
| $\lVert\cdot\rVert_2$ | Euclidean norm; its square sums squared entries. |
| $I$ | Identity matrix. |
| $\sigma^2$ | Error variance under a homoscedastic model. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Imagine fitting a flat sheet through a cloud of points. Each coefficient tilts the sheet in one feature direction. Least squares chooses the tilt that makes the combined vertical misses as small as possible, while the residuals are the arrows from the sheet to the observed points.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Expand $\lVert y-X\beta\rVert_2^2$, differentiate, and set the gradient to zero to obtain the normal equations.
- Use orthogonal projection to prove that the fitted vector lies in the column space of $X$ and the residual is perpendicular to that space.
- Prove convexity by showing the Hessian $2X^TX$ is positive semidefinite.

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

For $T$ full-batch gradient steps, a dominant dense cost is approximately:

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

## Advantages

### Explicit separation of layers

The pipeline distinguishes the model $y=X\beta+\varepsilon$, least-squares objective, numerical solver, library call, backend, and hardware. This prevents a change in software route from being mistaken for a change in mathematical estimator.

### Shape-aware route selection

Dense QR or SVD costs differ from sparse iterative work such as $O(T\operatorname{nnz}(X))$. Exposing matrix dimensions, rank, and sparsity makes solver choice an explicit engineering decision.

### Traceable numerical assumptions

A normal-equation route squares the condition number, while QR and SVD expose different stability and rank behavior. The pipeline makes those consequences visible rather than hiding them behind one API.

### Reproducible implementation mapping

Following the path from public API to low-level routine records where centering, weighting, tolerance, rank cutoff, precision, and device selection enter the computation.

### Comparable implementations

Two libraries can target the same least-squares objective through different solvers. A layered pipeline supports fair comparison of numerical accuracy, runtime, memory, and returned metadata.

## Limitations

### Pipeline is not a universal execution trace

Library versions, input types, sparse formats, options, and build configurations can change the actual branch. The diagram documents representative routes and must be checked against the installed version.

### Complexity remains conditional

Expressions such as $O(np^2)$ or $O(Tnp)$ omit constants, memory traffic, parallelism, convergence, and transfer cost. They are scaling models, not wall-clock promises.

### Hardware details are environment-specific

The same high-level call may use different BLAS vendors, thread counts, kernels, or devices. The mathematical result does not determine those runtime choices.

### Statistical validity is outside route correctness

A numerically accurate least-squares solve can still fit a misspecified, leaked, or shifted dataset. Computational correctness does not validate causal or inferential claims.

### Maintenance burden

Accurate source-to-backend mappings age as libraries change. Every asserted call path needs versioned evidence or periodic revalidation.

## Failure Modes

### Wrong branch assumptions

Treating a sparse input as if it used dense LAPACK, or an iterative framework fit as if it used a direct solve, produces incorrect complexity and stability claims.

### Silent rank or tolerance differences

Different solvers can classify small singular values differently, returning different coefficient vectors for nearly rank-deficient data while achieving similar residual norms.

### Precision and overflow mismatch

Reduced precision, poorly scaled features, or forming $X^TX$ can lose meaningful digits even when the high-level API reports success.

### Oversubscription and memory bottlenecks

Nested threading, copies, device transfers, or explicit formation of dense intermediates can dominate theoretical arithmetic cost and exhaust memory.

### Version drift

A documented internal function or backend may change after an upgrade. Reproducibility requires recording library, backend, compiler, and hardware versions.

### Layer collapse in reporting

Calling an API name “the algorithm” hides whether differences came from the model, objective, solver, numerical tolerance, backend, or device—the exact confusion this pipeline is meant to prevent.

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
