---
type: algorithm-family
name: Linear Models
members:
  - "[[Linear Regression]]"
  - "[[Ridge Regression]]"
  - "[[Lasso Regression]]"
  - "[[Elastic Net]]"
  - "[[Logistic Regression]]"
  - "[[Bayesian Linear Regression]]"
  - "[[Huber Regression]]"
status: reviewed
tags:
  - linear
  - parametric
  - interpretable
---

# Linear Models

## Definition

Linear models use a predictor that is linear in its unknown coefficients. For feature map:

$$
\phi(x)\in\mathbb{R}^{p}
$$

the linear score is:

$$
\eta(x)
=
\beta_0+\phi(x)^T\beta
$$

The features may include polynomials, interactions, splines, or other transformations. The family is linear because coefficients enter linearly, not because the raw input-to-output relationship must be a straight line.

## Unifying Principle

Members share a linear predictor but differ in at least one of the following:

- Target type.
- Observation model or link function.
- Residual or likelihood objective.
- Coefficient penalty.
- Frequentist or Bayesian treatment of parameters.
- Numerical procedure used for fitting.

These distinctions prevent the family, mathematical objective, solver, software implementation, backend, and hardware from being collapsed into one concept.

## Shared Mathematical Structure

For a design matrix:

$$
X\in\mathbb{R}^{n\times p}
$$

the vector of linear scores is:

$$
\eta
=
\beta_0\mathbf{1}+X\beta
$$

A regression model may use the score directly as a conditional mean. A generalized linear model transforms it through an inverse link. A regularized model adds a coefficient penalty. A Bayesian model places a prior over coefficients and computes a posterior.

## Members

| Algorithm | Task | Distinguishing feature |
|---|---|---|
| [[Linear Regression]] | [[Regression]] | Squared-residual estimation of a linear conditional mean |
| [[Ridge Regression]] | [[Regression]] | Squared coefficient penalty |
| [[Lasso Regression]] | [[Regression]] | Absolute-value coefficient penalty and sparse solutions |
| [[Elastic Net]] | [[Regression]] | Combined absolute and squared penalties |
| [[Huber Regression]] | [[Regression]] | Robust residual loss with bounded score |
| [[Logistic Regression]] | [[Classification]] | Logistic link and Bernoulli likelihood |
| [[Bayesian Linear Regression]] | [[Regression]] | Prior and posterior distributions over parameters |

## Shared Strengths

- Efficient prediction based on dot products.
- Coefficients can often be interpreted conditionally.
- Many fitting objectives are convex.
- Feature transformations allow substantial flexibility.
- Mature theory and implementations.

## Shared Limitations

- Misspecified feature representation can underfit nonlinear structure.
- Coefficients can be unstable under collinearity without regularization.
- Association is not causation.
- Extrapolation can be unreliable.
- Outliers, leverage, dependence, and distribution shift require explicit treatment.
- Preprocessing must be learned without validation or test leakage.

## Related Families

- [[Probabilistic Models]]
- [[Bayesian Methods]]
- [[Kernel Methods]]
- [[Neural Networks]]


