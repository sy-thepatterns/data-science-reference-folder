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

For targets $$y_i$$ and predictions $$\hat{y}_i$$:

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

both objectives have the same minimizer for fixed $$n$$.

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

Given predictions, computing the loss requires a pass over $$n$$ observations:

$$
O(n)
$$

with:

$$
O(1)
$$

additional working space when accumulated as a stream.
