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

Ridge is justified statistically through prediction risk, not because smaller coefficients are inherently better. Its advantage occurs when the variance removed by shrinkage exceeds the squared bias introduced by the penalty:

$$
\operatorname{MSE}(\hat\beta_\lambda)
=
\operatorname{Bias}(\hat\beta_\lambda)^2
+
\operatorname{Var}(\hat\beta_\lambda)
$$

### Strict convexity under positive penalization

For $\lambda>0$, the Hessian is

$$
2(X^TX+\lambda I)
$$

and for every nonzero $z$, $z^T(X^TX+\lambda I)z=\lVert Xz\rVert_2^2+\lambda\lVert z\rVert_2^2>0$. The objective therefore has a unique penalized solution even when $X$ is rank deficient.

### Stability along weak directions

Using $X=U\Sigma V^T$, ridge multiplies the ordinary least-squares contribution in singular direction $j$ by a shrinkage factor related to

$$
\frac{\sigma_j^2}{\sigma_j^2+\lambda}
$$

so directions with little data support are suppressed most strongly. This reduces sensitivity to collinearity and noise.

### Bias–variance trade-off

Ridge deliberately adds bias while often reducing estimator variance enough to lower predictive risk. The benefit is strongest when ordinary least-squares coefficients vary greatly across samples but the true signal is distributed across many features.

### Works when $p\ge n$

The positive penalty makes $X^TX+\lambda I$ invertible on penalized coordinates. A unique coefficient vector can therefore be estimated in high-dimensional settings where unregularized least squares is nonunique.

### Retains correlated predictors

Unlike an $L_1$ penalty, ridge usually shrinks correlated predictors together rather than selecting one and discarding the others. This can improve prediction when several measurements carry overlapping signal.

### Multiple numerical routes

Ridge may be solved as a linear system, augmented least-squares problem, SVD shrinkage, or iterative optimization problem. The route can be chosen separately according to sparsity, matrix shape, and required precision.

### Effective degrees of freedom are measurable

For the ridge smoother $S_\lambda=X(X^TX+\lambda I)^{-1}X^T$, a common complexity measure is

$$
\operatorname{df}(\lambda)
=
\operatorname{tr}(S_\lambda)
=
\sum_j\frac{\sigma_j^2}{\sigma_j^2+\lambda}
$$

This quantifies how regularization reduces variance continuously rather than by deleting predictors.

## Limitations

Every limitation below concerns a way the shrinkage assumption can be wrong, unstable, or mismeasured. The penalty changes the estimator and its estimand; it is not merely a numerical technique for solving ordinary least squares.

### No exact feature selection

The differentiable $L_2$ penalty pulls coefficients continuously toward zero but usually does not set them exactly to zero. Dense models retain storage, acquisition, and interpretation costs for every feature.

### Scale-dependent penalty

The penalty $\lambda\sum_j\beta_j^2$ acts on coefficient magnitude. Rescaling feature $x_j$ by $c$ rescales its equivalent coefficient by $1/c$ and changes its penalty contribution unless features or penalty factors are standardized appropriately.

### Isotropic shrinkage may be inappropriate

A single $\lambda I$ encodes equal prior or penalty strength in every coefficient direction. Features with different reliability, units, groups, or scientific roles may require structured penalties rather than uniform shrinkage.

### Regularization selection adds uncertainty

The chosen $\lambda$ depends on validation noise, split design, search grid, and metric. Reporting the final coefficients as though $\lambda$ were fixed in advance understates model-selection uncertainty.

### Biased coefficients

For $\lambda>0$, ridge coefficients are biased toward zero. That bias is the source of variance reduction but complicates direct effect interpretation and classical unpenalized inference.

### Squared-loss vulnerability

Ridge regularizes coefficients, not residual influence. The data-fit term still squares residuals, so vertical outliers can dominate the objective.

## Failure Modes

### Leakage during scaling or tuning

Standardizing before splitting or choosing $\lambda$ on the test set transmits evaluation information into the model and invalidates performance estimates.

### Penalty convention mismatch

Libraries may optimize $\lVert y-X\beta\rVert_2^2+\alpha\lVert\beta\rVert_2^2$, a mean-scaled loss, or another constant multiple. Copying a numerical penalty value across conventions changes the effective regularization.

### Penalizing the intercept unintentionally

Shrinking the intercept makes predictions depend on the arbitrary origin of the target and features. Most standard formulations center data or exclude the intercept from the penalty.

### Over-shrinkage and under-shrinkage

Very large $\lambda$ drives the model toward an intercept-only predictor; very small $\lambda$ recreates unstable ordinary least squares. A narrow or poorly designed search can miss the useful region.

### Sparse-signal mismatch

When only a few features truly matter and acquisition or interpretability requires selection, retaining every feature may be inefficient and obscure the relevant structure.

### Outliers and distribution shift

Large response errors, leverage points, and changes in feature support can still dominate or invalidate predictions. Coefficient regularization is not robust regression or drift protection.

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

