---
type: algorithm-family
name: Nearest-Neighbour Methods
parent_family: []
tasks: []
members:
  - "[[k-Nearest Neighbours]]"
  - "[[Radius Neighbours]]"
related:
  - "[[Kernel Methods]]"
  - "[[Clustering]]"
status: complete
tags:
  - nearest-neighbour-methods
---

# Nearest-Neighbour Methods

## Definition

Predict or estimate local structure from stored observations near a query under a chosen distance.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

For neighbourhood $$\mathcal{N}_k(x)$$, regression averages targets and classification votes: $$\hat y(x)=k^{-1}\sum_{i\in\mathcal{N}_k(x)}y_i$$.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[k-Nearest Neighbours]]
- [[Radius Neighbours]]

## Family Tree

```text
Nearest-Neighbour Methods
├── k-Nearest Neighbours
├── Radius Neighbours
└── Radius Neighbours
```

## Shared Strengths

Simple, nonparametric, naturally multiclass, and no parametric training step.

## Shared Limitations

Prediction and storage scale with data; distance concentration, feature scaling, irrelevant dimensions, imbalance, and index degradation in high dimensions.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[k-Nearest Neighbours]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Radius Neighbours]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Kernel Methods]]
- [[Clustering]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
