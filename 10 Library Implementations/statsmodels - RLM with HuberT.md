---
type: implementation
name: statsmodels - RLM with HuberT
algorithm:
  - "[[Huber Regression]]"
library:
  - "[[statsmodels]]"
status: reviewed
tags:
  - huber
  - implementation
---

# statsmodels - RLM with HuberT

## Implements

A native robust linear model for [[Huber Regression]], whose defining objective is a linear predictor fitted with Huber residual loss.

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
| $r_i$ | Residual for example $i$: observed minus predicted value. |
| $\sigma$ | Positive residual scale used to make errors comparable. |
| $u_i=r_i/\sigma$ | Standardized residual. |
| $\delta$ | Positive cutoff between quadratic treatment of small errors and linear treatment of large errors. |
| $\rho_\delta$ | Huber loss. |
| $\psi_\delta$ | Derivative or score of the Huber loss. |
| $w_i$ | Robust weight assigned to example $i$ in an iteratively reweighted solver. |
| $R(\beta,\sigma)$ | Estimator-specific regularization or scale term. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Small mistakes are treated like squared error because they are useful for fine adjustment. Once a mistake is very large, Huber loss stops letting it dominate the lesson. It still counts the mistake, but its influence grows like a straight line instead of exploding like a square.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Check that the quadratic and linear pieces of $\rho_\delta$ meet with the same value and slope at $|u|=\delta$.
- Differentiate each piece to obtain the bounded score function $\psi_\delta$.
- Derive the iteratively reweighted form from $w_i=\psi(u_i)/u_i$ for nonzero residuals.

## Public API

```python
model = sm.RLM(y, X1, M=sm.robust.norms.HuberT())
res = model.fit()
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `statsmodels.RLM and statsmodels.robust.norms.HuberT` |
| Fitting style | M-estimation with robust scale and iteratively reweighted fitting |
| Core solver route | IRLS |
| Statistical inference | Robust-model result summaries |
| Sparse support | Limited |
| GPU support | No |

## Objective Mapping

The intended mathematical target is a linear predictor fitted with Huber residual loss. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
M-estimation with robust scale and iteratively reweighted fitting
    ↓
IRLS
    ↓
statsmodels numerical operations and dependencies
    ↓
available CPU or accelerator backend
```

## Complexity Variables

$$
n=\text{number of samples}
$$

$$
p=\text{number of features}
$$

$$
T=\text{number of solver iterations or training passes}
$$

$$
b=\text{mini-batch size}
$$

## Training Complexity

Representative time:

$$
\text{O(T(np^2 + p^3)) for dense weighted solves}
$$

Representative additional or active space:

$$
\text{O(np + p^2)}
$$

These are route-level summaries, not universal bounds. Data shape, sparsity, active-set size, precision, convergence tolerance, line searches, batching, and linked numerical libraries can change actual cost.

## Prediction Complexity

For a dense fitted coefficient vector and:

$$
m=\text{number of prediction rows}
$$

prediction is dominated by a matrix-vector product:

$$
O(mp)
$$

with output storage:

$$
O(m)
$$

Sparse learned coefficients or sparse inputs can reduce arithmetic when the implementation exploits them.

## Numerical and Statistical Caveats

Its scale, norm, covariance, and stopping definitions differ from scikit-learn's HuberRegressor.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Huber Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official statsmodels documentation for the named API or building blocks.
- [[Huber Regression]]
- [[Huber Regression Implementation Comparison]]
