---
type: loss-function
name: Mean Squared Error
aliases:
  - MSE
tasks:
  - "[[Regression]]"
algorithms:
  - "[[Linear Regression]]"
related:
  - "[[Root Mean Squared Error]]"
status: reviewed
tags:
  - regression
  - differentiable
  - convex
---

# Mean Squared Error

## Formal Definition

For targets $y_i$ and predictions $\hat{y}_i$:

$$
\operatorname{MSE}
=
\frac{1}{n}
\sum_{i=1}^{n}
\left(y_i-\hat{y}_i\right)^2
$$

In vector form:

$$
\operatorname{MSE}
=
\frac{1}{n}
\lVert y-\hat{y}\rVert_2^2
$$

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

## Relationship to Residual Sum of Squares

Residual sum of squares is:

$$
\operatorname{RSS}
=
\lVert y-\hat{y}\rVert_2^2
$$

Since:

$$
\operatorname{MSE}
=
\frac{1}{n}\operatorname{RSS}
$$

both objectives have the same minimizer for fixed $n$.

## Gradient for Linear Regression

With:

$$
\hat{y}=X\beta
$$

the gradient is:

$$
\nabla_\beta \operatorname{MSE}
=
\frac{2}{n}
X^{T}(X\beta-y)
$$

## Hessian

$$
\nabla_\beta^2 \operatorname{MSE}
=
\frac{2}{n}X^{T}X
$$

The Hessian is positive semidefinite, so the objective is convex.

## Complexity

Given predictions, computing the loss requires a pass over $n$ observations:

$$
O(n)
$$

with:

$$
O(1)
$$

additional working space when accumulated as a stream.
