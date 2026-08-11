---
type: algorithm-family
name: Bayesian Methods
parent_family: []
tasks: []
members:
  - "[[Bayesian Linear Regression]]"
  - "[[Gaussian Process]]"
related:
  - "[[Probabilistic Models]]"
  - "[[Linear Models]]"
status: complete
tags:
  - bayesian-methods
---

# Bayesian Methods

## Definition

Use probability distributions to represent uncertainty about unknown quantities and update them by conditioning on observed data.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

$$p(\theta\mid\mathcal{D})\propto p(\mathcal{D}\mid\theta)p(\theta)$$, with predictions obtained by posterior integration.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Bayesian Linear Regression]]
- [[Gaussian Process]]

## Family Tree

```text
Bayesian Methods
├── Bayesian Linear Regression
├── Gaussian Process
└── Gaussian Process
```

## Shared Strengths

Explicit uncertainty, prior information, hierarchical structure, and coherent sequential updating.

## Shared Limitations

Misspecification and prior sensitivity remain; exact inference is often unavailable, and approximation diagnostics are essential.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Bayesian Linear Regression]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Gaussian Process]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Probabilistic Models]]
- [[Linear Models]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
