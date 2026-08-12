---
type: implementation
name: SciPy - Lasso Regression
algorithm:
  - "[[Lasso Regression]]"
library:
  - "[[SciPy]]"
status: reviewed
tags:
  - lasso
  - implementation
---

# SciPy - Lasso Regression

## Implements

A custom construction; no dedicated estimator for [[Lasso Regression]], whose defining objective is squared residual loss plus an L1 coefficient penalty.

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
| $\lambda$ | Nonnegative overall penalty strength. |
| $\lVert\beta\rVert_1$ | Sum of absolute coefficient values; the lasso penalty. |
| $\partial$ | Subdifferential: the set of valid slopes at a nondifferentiable point. |
| $S(z,\lambda)$ | Soft-thresholding operator, which moves $z$ toward zero and may set it exactly to zero. |
| $s=\lVert\hat\beta\rVert_0$ | Number of fitted nonzero coefficients; $\lVert\cdot\rVert_0$ is counting notation, not a true norm. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Picture each coefficient attached to a rubber band pulling it toward zero. The lasso's absolute-value band has a sharp corner at zero, so weak coefficients can stick there exactly. That is why lasso can remove features rather than merely making every coefficient smaller.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Derive the coordinate update by holding all other coefficients fixed and minimizing the resulting one-variable quadratic-plus-absolute-value problem.
- Use the subgradient optimality condition to prove the threshold rule for a zero coefficient.
- Explain sparsity geometrically by drawing elliptical loss contours touching the corners of an $L_1$ constraint diamond.

## Public API

```python
beta = soft_threshold(beta - step * X.T @ (X @ beta - y) / n, step * alpha)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `scipy sparse operations plus a custom proximal solver` |
| Fitting style | Proximal gradient, coordinate descent, or generic constrained reformulation |
| Core solver route | User-selected optimization routine |
| Statistical inference | None automatic |
| Sparse support | Yes for custom operations |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L1 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Proximal gradient, coordinate descent, or generic constrained reformulation
    ↓
User-selected optimization routine
    ↓
SciPy numerical operations and dependencies
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
\text{O(T nnz(X)) sparse high-level}
$$

Representative additional or active space:

$$
\text{O(nnz(X) + p)}
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

A generic smooth `minimize` call is not a faithful treatment of the nondifferentiable L1 kink unless the problem is reformulated or a suitable method is supplied.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Lasso Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official SciPy documentation for the named API or building blocks.
- [[Lasso Regression]]
- [[Lasso Regression Implementation Comparison]]
