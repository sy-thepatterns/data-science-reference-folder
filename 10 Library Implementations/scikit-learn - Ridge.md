---
type: implementation
name: scikit-learn - Ridge
algorithm:
  - "[[Ridge Regression]]"
library:
  - "[[scikit-learn]]"
status: reviewed
tags:
  - ridge
  - implementation
---

# scikit-learn - Ridge

## Implements

A native estimator for [[Ridge Regression]], whose defining objective is squared residual loss plus an L2 coefficient penalty.

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
| $\lambda$ | Nonnegative penalty strength; larger values shrink coefficients more strongly. |
| $I$ | Identity matrix, with ones on its diagonal and zeros elsewhere. |
| $\lVert\beta\rVert_2^2$ | Sum of squared coefficients. |
| $z$ | Arbitrary nonzero direction used to test positive definiteness. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

If several features tell nearly the same story, ordinary least squares can give them huge positive and negative coefficients that cancel. Ridge charges for large coefficients, so it prefers a calmer solution that makes almost the same predictions without those wild cancellations.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Differentiate squared residual loss plus $\lambda\beta^T\beta$ and set the gradient to zero to obtain the ridge normal equations.
- Prove uniqueness for $\lambda>0$ by showing $z^T(X^TX+\lambda I)z=\lVert Xz\rVert_2^2+\lambda\lVert z\rVert_2^2>0$.
- Derive the augmented least-squares form by expanding the norm of the vertically stacked system.

## Public API

```python
from sklearn.linear_model import Ridge
model = Ridge(alpha=1.0).fit(X, y)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `sklearn.linear_model.Ridge` |
| Fitting style | Solver-dispatch estimator; dense, sparse, and iterative routes |
| Core solver route | auto selects among direct and iterative solvers |
| Statistical inference | Limited |
| Sparse support | Yes, solver-dependent |
| GPU support | No standard route |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L2 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Solver-dispatch estimator; dense, sparse, and iterative routes
    ↓
auto selects among direct and iterative solvers
    ↓
scikit-learn numerical operations and dependencies
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
\text{Direct dense: O(np^2 + p^3); iterative: O(T nnz(X))}
$$

Representative additional or active space:

$$
\text{O(np + p^2) dense, route-dependent}
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

The meaning of `alpha` follows scikit-learn's unnormalized residual-sum convention; solver choice changes complexity and numerical behaviour.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Ridge Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official scikit-learn documentation for the named API or building blocks.
- [[Ridge Regression]]
- [[Ridge Regression Implementation Comparison]]
