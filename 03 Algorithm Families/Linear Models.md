---
type: algorithm-family
name: Linear Models
parent_family: []
tasks: []
members:
  - "[[Linear Regression]]"
  - "[[Ridge Regression]]"
  - "[[Lasso Regression]]"
  - "[[Elastic Net]]"
  - "[[Logistic Regression]]"
  - "[[Bayesian Linear Regression]]"
  - "[[Huber Regression]]"
related:
  - "[[Kernel Methods]]"
  - "[[Probabilistic Models]]"
  - "[[Bayesian Methods]]"
status: complete
tags:
  - linear-models
---

# Linear Models

## Definition

Use predictors linear in unknown coefficients, even when features are nonlinear transformations of raw inputs.

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
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Read the formula as a recipe: identify what is observed, what must be learned, and what quantity says whether an answer is good. The notation compresses that recipe, but it does not turn the model into a solver or a software package.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Expand the stated objective from its definition and check that every term has compatible dimensions.
- Derive the first-order condition by differentiating or using a subgradient when the objective has a sharp corner.
- Check special cases and boundary values to confirm that the formula reduces to simpler known results.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

With feature map $\phi(x)$, the score is $\eta(x)=\beta_0+\phi(x)^T\beta$; members differ in link, likelihood, loss, penalty, and parameter treatment.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Linear Regression]]
- [[Ridge Regression]]
- [[Lasso Regression]]
- [[Elastic Net]]
- [[Logistic Regression]]
- [[Bayesian Linear Regression]]
- [[Huber Regression]]

## Family Tree

```text
Linear Models
├── Linear Regression
├── Ridge Regression
└── Huber Regression
```

## Shared Strengths

Fast prediction, mature theory, many convex fitting problems, and conditional interpretability.

## Shared Limitations

Feature misspecification, collinearity, unreliable extrapolation, leverage, and association being mistaken for causation.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Linear Regression]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Huber Regression]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Kernel Methods]]
- [[Probabilistic Models]]
- [[Bayesian Methods]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
