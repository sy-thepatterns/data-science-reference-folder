---
type: algorithm
name: Ridge Regression
aliases:
  - Tikhonov-Regularized Linear Regression
learning_paradigm:
  - "[[Supervised Learning]]"
task:
  - "[[Regression]]"
family:
  - "[[Linear Models]]"
foundations:
  - "[[Linear Regression]]"
  - "[[Euclidean Norm]]"
objective:
  - "[[Least Squares]]"
loss:
  - "[[Mean Squared Error]]"
optimization:
  - L2-regularized least squares
solvers:
  - "[[Normal Equations]]"
  - "[[Singular Value Decomposition]]"
implementations:
  - "[[scikit-learn - Ridge]]"
  - "[[statsmodels - Ridge via fit_regularized]]"
  - "[[NumPy - Ridge Regression]]"
  - "[[SciPy - Ridge Regression]]"
  - "[[PyTorch - Ridge Regression]]"
  - "[[TensorFlow - Ridge Regression]]"
related:
  - "[[Lasso Regression]]"
  - "[[Elastic Net]]"
  - "[[Bayesian Linear Regression]]"
status: reviewed
tags:
  - linear
  - parametric
  - frequentist
  - convex
  - differentiable
  - interpretable
  - regularized
---

# Ridge Regression

## Overview

Ridge regression is a regularized version of [[Linear Regression]]. It minimizes squared residual error while penalizing the squared Euclidean norm of the coefficient vector. The penalty shrinks coefficients toward zero, reduces variance, and makes estimation unique even when the design matrix is rank deficient, provided the penalized coefficients receive a strictly positive penalty.

The intercept is normally excluded from the penalty. Feature scaling matters because the penalty depends on coefficient magnitude.

## Problem Definition

Given:

$$
X\in\mathbb{R}^{n\times p}
$$

and:

$$
y\in\mathbb{R}^{n}
$$

estimate coefficients:

$$
\hat{\beta}\in\mathbb{R}^{p}
$$

and optionally an intercept:

$$
\hat{\beta}_0\in\mathbb{R}
$$

## Formal Definition

For penalty strength:

$$
\lambda\ge 0
$$

ridge solves:

$$
\hat{\beta}_{\lambda}
=
\arg\min_{\beta}
\left\{
\lVert y-X\beta\rVert_2^2
+
\lambda\lVert\beta\rVert_2^2
\right\}
$$

Some sources divide the residual term by the sample count or by two. Those conventions change the numerical meaning of the regularization parameter but not the model class.

## Derivation

The objective is:

$$
L(\beta)
=
(y-X\beta)^T(y-X\beta)
+
\lambda\beta^T\beta
$$

Its gradient is:

$$
\nabla_{\beta}L
=
-2X^Ty
+
2X^TX\beta
+
2\lambda\beta
$$

Setting the gradient to zero gives the ridge normal equations:

$$
(X^TX+\lambda I)\hat{\beta}_{\lambda}
=
X^Ty
$$

When the penalized coefficients all receive a positive penalty:

$$
\hat{\beta}_{\lambda}
=
(X^TX+\lambda I)^{-1}X^Ty
$$

This inverse is a mathematical expression. Numerical implementations solve the linear system without explicitly forming an inverse.

## Why the Penalty Stabilizes the Problem

The Hessian is:

$$
\nabla_{\beta}^2L
=
2(X^TX+\lambda I)
$$

For any nonzero vector:

$$
z^T(X^TX+\lambda I)z
=
\lVert Xz\rVert_2^2+\lambda\lVert z\rVert_2^2
>0
$$

when the penalty is positive. The objective is then strictly convex and has a unique minimizer.

## Statistical Properties

### Bias and Variance

Ridge generally introduces coefficient bias. In exchange, it can substantially reduce variance, especially when predictors are highly correlated or when the feature count is large relative to the sample count.

### Sparsity

The squared penalty usually shrinks coefficients without setting them exactly to zero. Ridge is therefore not a feature-selection method in the same sense as [[Lasso Regression]].

### Bayesian Interpretation

With a Gaussian likelihood and an isotropic zero-mean Gaussian prior on coefficients, the maximum a posteriori estimate has ridge form. [[Bayesian Linear Regression]] additionally represents the full posterior distribution rather than only its mode.

## Optimization and Solvers

Ridge regression is the model and objective. Cholesky-like linear-system solves, SVD-based methods, conjugate-gradient methods, stochastic gradient methods, and coordinate methods are distinct numerical procedures that may fit it.

An augmented least-squares form is:

$$
\left\lVert
\begin{bmatrix}
y\\
0
\end{bmatrix}
-
\begin{bmatrix}
X\\
\sqrt{\lambda}I
\end{bmatrix}
\beta
\right\rVert_2^2
$$

This form permits QR- or SVD-based least-squares solvers.

## Training Pseudocode

```text
INPUT:
    design matrix X
    target y
    penalty lambda
    fit_intercept flag
    selected ridge solver

1. Validate shapes and lambda.
2. Center X and y when fitting an unpenalized intercept.
3. Scale features when coefficient comparability is intended.
4. Solve the L2-regularized least-squares objective.
5. Recover the intercept from the offsets.
6. Store coefficients and preprocessing metadata.

OUTPUT:
    coefficients and intercept
```

## Complexity

There is no solver-independent training complexity. For a dense design with more rows than columns, forming the Gram matrix costs:

$$
O(np^2)
$$

and solving the resulting system costs:

$$
O(p^3)
$$

An iterative sparse route often has a dominant per-iteration cost proportional to:

$$
O(\operatorname{nnz}(X))
$$

Prediction for a dense batch of size:

$$
m
$$

costs:

$$
O(mp)
$$

## Hyperparameters

| Parameter | Mathematical effect | Typical behaviour |
|---|---|---|
| Penalty strength | Controls coefficient shrinkage | Larger values increase bias and usually reduce variance |
| Fit intercept | Adds an unpenalized constant | Avoids forcing predictions through the origin |
| Feature scaling | Changes the effective penalty by feature | Standardization makes shrinkage more comparable |
| Solver tolerance | Controls iterative stopping | Tighter values increase work and numerical accuracy |

## Advantages

- Convex and, under positive penalization, strictly convex.
- Stabilizes coefficients under multicollinearity.
- Handles more features than observations.
- Retains all predictors and has efficient prediction.

## Limitations and Failure Modes

- Does not usually produce sparse coefficients.
- Requires regularization selection, commonly by cross-validation.
- Coefficients depend on feature scaling.
- A single isotropic penalty may over-shrink important directions.
- Squared residual loss remains sensitive to large response outliers.
- Leakage during scaling or regularization selection produces optimistic evaluation.

## Diagnostics

- Compare validation error across a logarithmic penalty grid.
- Inspect coefficient paths and effective model complexity.
- Perform scaling inside each training fold.
- Examine residual structure and influential observations.
- Compare against unregularized and sparse alternatives.

## Related Algorithms

- [[Linear Regression]] is recovered when the penalty is zero.
- [[Lasso Regression]] uses an absolute-value penalty and can produce exact zeros.
- [[Elastic Net]] combines squared and absolute-value penalties.
- [[Bayesian Linear Regression]] can yield ridge as a posterior-mode estimate under Gaussian assumptions.

## Implementations

- [[scikit-learn - Ridge]]
- [[statsmodels - Ridge via fit_regularized]]
- [[NumPy - Ridge Regression]]
- [[SciPy - Ridge Regression]]
- [[PyTorch - Ridge Regression]]
- [[TensorFlow - Ridge Regression]]
- [[Ridge Regression Implementation Comparison]]

## References

- Hoerl, A. E., and Kennard, R. W. (1970). *Ridge Regression: Biased Estimation for Nonorthogonal Problems*.
- Hastie, T., Tibshirani, R., and Friedman, J. *The Elements of Statistical Learning*.
- [[Linear Regression]]
- [[Least Squares]]

