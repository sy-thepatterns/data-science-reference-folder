---
type: algorithm-family
name: Ensemble Methods
parent_family: []
tasks: []
members:
  - "[[Bagging]]"
  - "[[Random Forest]]"
  - "[[Extra Trees]]"
  - "[[Gradient Boosting]]"
  - "[[Stacking]]"
related:
  - "[[Tree-Based Methods]]"
  - "[[Linear Models]]"
  - "[[Neural Networks]]"
status: complete
tags:
  - ensemble-methods
---

# Ensemble Methods

## Definition

Combine multiple fitted predictors into one prediction rule.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

For base predictors $$f_m$$, aggregation forms $$F(x)=\sum_m w_m f_m(x)$$ or a vote; sequential boosting instead fits new members to improve the current ensemble.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Bagging]]
- [[Random Forest]]
- [[Extra Trees]]
- [[Gradient Boosting]]
- [[Stacking]]

## Family Tree

```text
Ensemble Methods
├── Bagging
├── Random Forest
└── Stacking
```

## Shared Strengths

Often improves accuracy and stability and can reduce variance or bias.

## Shared Limitations

More compute and memory, harder interpretation, correlated errors, leakage-prone stacking, and poor extrapolation inherited from members.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Bagging]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Stacking]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Tree-Based Methods]]
- [[Linear Models]]
- [[Neural Networks]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
