---
type: algorithm-family
name: Tree-Based Methods
parent_family: []
tasks: []
members:
  - "[[Decision Tree]]"
  - "[[Random Forest]]"
  - "[[Extra Trees]]"
  - "[[Gradient Boosting]]"
  - "[[XGBoost]]"
  - "[[LightGBM]]"
  - "[[CatBoost]]"
related:
  - "[[Ensemble Methods]]"
  - "[[Nearest-Neighbour Methods]]"
status: complete
tags:
  - tree-based-methods
---

# Tree-Based Methods

## Definition

Partition feature space recursively and predict with region-specific constants or simple models.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

A split $$(j,t)$$ divides a node into $$R_L=\{x:x_j\le t\}$$ and $$R_R$$ to reduce impurity or loss; ensembles aggregate or sequentially add trees.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Decision Tree]]
- [[Random Forest]]
- [[Extra Trees]]
- [[Gradient Boosting]]
- [[XGBoost]]
- [[LightGBM]]
- [[CatBoost]]

## Family Tree

```text
Tree-Based Methods
├── Decision Tree
├── Random Forest
└── CatBoost
```

## Shared Strengths

Handles nonlinear interactions, mixed scales, missing-value strategies, little scaling, and strong tabular performance.

## Shared Limitations

Piecewise-constant extrapolation, instability of single trees, biased importance measures, overfitting, and implementation-specific categorical or missing-value behaviour.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Decision Tree]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[CatBoost]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Ensemble Methods]]
- [[Nearest-Neighbour Methods]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
