---
type: algorithm
name: Lasso Regression
aliases:
  - Lasso
  - Least Absolute Shrinkage and Selection Operator
learning_paradigm:
  - "[[Supervised Learning]]"
task:
  - "[[Regression]]"
family:
  - "[[Linear Models]]"
foundations:
  - "[[Linear Regression]]"
objective:
  - L1-regularized least squares
loss:
  - "[[Mean Squared Error]]"
optimization:
  - Convex nonsmooth optimization
solvers:
  - Coordinate Descent
  - Least-Angle Regression
  - Proximal Gradient Method
implementations:
  - "[[scikit-learn - Lasso]]"
  - "[[statsmodels - Lasso via fit_regularized]]"
  - "[[NumPy - Lasso Regression]]"
  - "[[SciPy - Lasso Regression]]"
  - "[[PyTorch - Lasso Regression]]"
  - "[[TensorFlow - Lasso Regression]]"
related:
  - "[[Ridge Regression]]"
  - "[[Elastic Net]]"
status: reviewed
tags:
  - linear
  - parametric
  - frequentist
  - convex
  - nondifferentiable
  - interpretable
  - regularized
  - sparse
  - high-dimensional
---

# Lasso Regression

## Overview

Lasso regression is a regularized version of [[Linear Regression]] that adds an absolute-value coefficient penalty. The geometry of the penalty can set coefficients exactly to zero, so fitting and feature selection occur together.

The intercept is normally unpenalized. Feature scaling is important because rescaling a feature changes the coefficient magnitude needed to represent the same prediction.

## Formal Definition

For:

$$
\lambda\ge 0
$$

lasso solves:

$$
\hat{\beta}_{\lambda}
=
\arg\min_{\beta}
\left\{
\frac{1}{2n}\lVert y-X\beta\rVert_2^2
+
\lambda\lVert\beta\rVert_1
\right\}
$$

where:

$$
\lVert\beta\rVert_1
=
\sum_{j=1}^{p}|\beta_j|
$$

Equivalent scaling conventions appear in software and literature. A regularization value must therefore be interpreted together with the exact objective convention.

## Optimality Conditions

The absolute-value penalty is not differentiable at zero. Its subgradient for coefficient:

$$
\beta_j
$$

is:

$$
\partial|\beta_j|
=
\begin{cases}
\{1\}, & \beta_j>0\\
[-1,1], & \beta_j=0\\
\{-1\}, & \beta_j<0
\end{cases}
$$

The optimum satisfies:

$$
0
\in
\frac{1}{n}X^T(X\hat{\beta}-y)
+
\lambda\partial\lVert\hat{\beta}\rVert_1
$$

For a zero coefficient, this permits:

$$
\left|
\frac{1}{n}x_j^T(y-X\hat{\beta})
\right|
\le
\lambda
$$

This threshold condition explains how predictors can be excluded exactly.

## Coordinate Update

With standardized features and partial residual:

$$
r_j
=
y-\sum_{k\ne j}x_k\beta_k
$$

a coordinate-descent update uses soft thresholding:

$$
\beta_j
\leftarrow
\frac{
S\left(\frac{1}{n}x_j^Tr_j,\lambda\right)
}{
\frac{1}{n}x_j^Tx_j
}
$$

where:

$$
S(z,\lambda)
=
\operatorname{sign}(z)
\max(|z|-\lambda,0)
$$

Coordinate descent is a solver for the lasso objective; it is not the lasso model itself.

## Statistical Properties

### Bias and Sparsity

Lasso shrinks nonzero coefficients and therefore introduces bias. It can reduce variance and produce a sparse representation when the signal is sufficiently concentrated.

### Correlated Predictors

When predictors are strongly correlated, lasso may select one and discard others in an unstable way. [[Elastic Net]] often behaves more smoothly for correlated groups.

### Identifiability

The fitted prediction may be unique under conditions weaker than those required for a unique coefficient vector. Exact uniqueness depends on the design matrix and active set.

### Model-Selection Consistency

Sparsity alone does not guarantee recovery of the true support. Consistent variable selection requires additional assumptions on the design, signal strength, and regularization sequence.

## Training Pseudocode

```text
INPUT:
    design matrix X
    target y
    penalty lambda
    convergence tolerance
    maximum iterations

1. Validate data and penalty.
2. Center and scale features using training data.
3. Initialize coefficients, commonly at zero or from a nearby path value.
4. Repeatedly update coefficients with the chosen convex solver.
5. Check an objective, dual-gap, or parameter-change criterion.
6. Recover the unpenalized intercept.

OUTPUT:
    sparse coefficient vector and intercept
```

## Complexity

Complexity depends on solver, sparsity, conditioning, tolerance, and warm starts. A dense coordinate sweep has cost approximately:

$$
O(np)
$$

With:

$$
T
$$

sweeps, a common upper-level description is:

$$
O(Tnp)
$$

Sparse implementations can depend instead on the nonzero count. Prediction for:

$$
s=\lVert\hat{\beta}\rVert_0
$$

active coefficients and a batch of size:

$$
m
$$

can cost:

$$
O(ms)
$$

when the active representation is exploited.

## Hyperparameters

| Parameter | Mathematical effect | Typical behaviour |
|---|---|---|
| Penalty strength | Controls shrinkage and sparsity | Larger values yield more zero coefficients |
| Tolerance | Defines numerical stopping | Tighter values require more iterations |
| Maximum iterations | Caps solver work | Too small a value can leave a nonconverged estimate |
| Feature scaling | Sets relative penalty across features | Standardization makes penalties comparable |

## Advantages

- Produces sparse, potentially interpretable models.
- Convex objective with efficient specialized solvers.
- Useful when the feature count is large.
- Can reduce prediction variance and storage cost.

## Limitations and Failure Modes

- Biased estimates for large true coefficients.
- Selection instability among correlated predictors.
- Feature scaling materially affects the solution.
- Cross-validation must include preprocessing inside each fold.
- Exact zeros are not proof of scientific irrelevance.
- Post-selection uncertainty is not captured by ordinary OLS formulas.
- Squared residual loss remains sensitive to response outliers.

## Diagnostics

- Plot validation error and active-feature count over a regularization path.
- Verify solver convergence, preferably with a dual gap when available.
- Assess selection stability across resamples.
- Compare lasso with ridge and elastic net.
- Inspect residuals and influential observations.

## Related Algorithms

- [[Linear Regression]] is the unpenalized baseline.
- [[Ridge Regression]] shrinks continuously but rarely selects exactly.
- [[Elastic Net]] adds a squared penalty and can stabilize correlated groups.

## Implementations

- [[scikit-learn - Lasso]]
- [[statsmodels - Lasso via fit_regularized]]
- [[NumPy - Lasso Regression]]
- [[SciPy - Lasso Regression]]
- [[PyTorch - Lasso Regression]]
- [[TensorFlow - Lasso Regression]]
- [[Lasso Regression Implementation Comparison]]

## References

- Tibshirani, R. (1996). *Regression Shrinkage and Selection via the Lasso*.
- Hastie, T., Tibshirani, R., and Wainwright, M. *Statistical Learning with Sparsity*.
- [[Mean Squared Error]]

