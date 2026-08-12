---
type: comparison
name: Linear Regression Implementation Comparison
compares:
  - "[[scikit-learn - LinearRegression]]"
  - "[[statsmodels - OLS]]"
  - "[[NumPy - lstsq]]"
  - "[[SciPy - lstsq]]"
  - "[[PyTorch - Linear Regression]]"
  - "[[TensorFlow - Linear Regression]]"
status: reviewed
tags:
  - comparison
  - linear
---

# Linear Regression Implementation Comparison

| Property              | scikit-learn                           | statsmodels                               | NumPy                                      | SciPy                                  | PyTorch                                 | TensorFlow                              |
| --------------------- | -------------------------------------- | ----------------------------------------- | ------------------------------------------ | -------------------------------------- | --------------------------------------- | --------------------------------------- |
| Primary role          | ML estimator                           | Statistical model and inference           | Direct array solve                         | Direct scientific least-squares solve  | Differentiable model training           | Differentiable model training           |
| Default fitting style | Direct dense or iterative sparse solve | Least-squares model fit                   | Direct dense solve                         | Direct dense solve with driver control | Usually iterative gradient optimization | Usually iterative gradient optimization |
| Intercept             | Managed by estimator                   | Usually added explicitly                  | User constructs it                         | User constructs it                     | Bias parameter in layer                 | Bias parameter in layer                 |
| Statistical inference | Limited                                | Extensive                                 | None                                       | None                                   | Not automatic                           | Not automatic                           |
| Pipelines             | Strong sklearn integration             | Formula/data-model ecosystem              | Manual                                     | Manual                                 | Neural-module ecosystem                 | Keras/TensorFlow ecosystem              |
| Sparse input          | Supported through sparse route         | Model-dependent                           | Dense `linalg` interface                   | Separate sparse solvers available      | Sparse support is operation-specific    | Sparse support is operation-specific    |
| GPU                   | Not the standard route                 | No standard GPU estimator route           | Depends on NumPy environment; normally CPU | Normally CPU                           | Yes                                     | Yes                                     |
| Rank diagnostics      | Dense attributes                       | Available through results/model internals | Returns rank and singular values           | Returns rank and singular values       | Not automatic                           | Not automatic                           |
| Solver control        | Limited by estimator route             | Fit-method dependent                      | `rcond`                                    | LAPACK driver and `cond`               | Optimizer chosen by user                | Optimizer chosen by user                |
| Best use              | Predictive tabular ML                  | Inference and diagnostics                 | Minimal direct matrix solve                | Controlled dense least squares         | Embedded differentiable systems         | TensorFlow/Keras systems                |

## Decision Guide

### Choose scikit-learn when

- You need preprocessing pipelines.
- You need cross-validation and estimator composition.
- Prediction is the primary goal.
- You want automatic intercept handling and a conventional ML API.

### Choose statsmodels when

- Coefficient inference matters.
- You need standard errors, tests, confidence intervals, and diagnostics.
- You are building an explicit statistical model.

### Choose NumPy when

- You need a minimal direct least-squares call.
- You will handle intercepts, preprocessing, and evaluation yourself.

### Choose SciPy when

- You need direct control over dense least-squares drivers.
- Rank and singular-value diagnostics matter.
- You are working close to numerical linear algebra.

### Choose PyTorch or TensorFlow when

- The linear model is part of a larger differentiable architecture.
- You need custom losses or training loops.
- You need accelerator execution.
- An iterative solution is acceptable or desirable.

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

## Complexity Warning

Do not compare these packages using a single universal Big-O value.

Direct dense solvers are factorization-based:

$$
O\left(
\min\left(np^2,n^2p\right)
\right)
$$

for representative SVD-like routes.

Iterative framework training is approximately:

$$
O(Tnp)
$$

for $T$ full-batch updates and one output.

Sparse iterative routes are often summarized by:

$$
O\left(T\operatorname{nnz}(X)\right)
$$

The constants, memory, precision, convergence, and numerical robustness differ substantially.
