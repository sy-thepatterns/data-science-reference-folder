---
type: algorithm-family
name: Kernel Methods
parent_family: []
tasks: []
members:
  - "[[Support Vector Machine]]"
  - "[[Support Vector Regression]]"
  - "[[Kernel Ridge Regression]]"
  - "[[Gaussian Process]]"
related:
  - "[[Linear Models]]"
  - "[[Nearest-Neighbour Methods]]"
status: complete
tags:
  - kernel-methods
---

# Kernel Methods

## Definition

Use positive-semidefinite similarity functions to fit linear methods in an implicit feature space.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

A kernel satisfies $$k(x,x')=\langle\phi(x),\phi(x')\rangle$$; training often depends on Gram matrix $$K_{ij}=k(x_i,x_j)$$.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Support Vector Machine]]
- [[Support Vector Regression]]
- [[Kernel Ridge Regression]]
- [[Gaussian Process]]

## Family Tree

```text
Kernel Methods
├── Support Vector Machine
├── Support Vector Regression
└── Gaussian Process
```

## Shared Strengths

Convex objectives for many members, flexible nonlinear boundaries, and principled similarity design.

## Shared Limitations

Dense Gram matrices require $$O(n^2)$$ storage and often $$O(n^3)$$ factorization; kernel and hyperparameter choice dominate behaviour.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Support Vector Machine]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Gaussian Process]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Linear Models]]
- [[Nearest-Neighbour Methods]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
