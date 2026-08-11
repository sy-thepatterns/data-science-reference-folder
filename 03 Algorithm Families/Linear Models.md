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

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

With feature map $$\phi(x)$$, the score is $$\eta(x)=\beta_0+\phi(x)^T\beta$$; members differ in link, likelihood, loss, penalty, and parameter treatment.

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
