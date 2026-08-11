---
type: algorithm-family
name: Graph Learning Methods
parent_family: []
tasks: []
members:
  - "[[Graph Neural Network]]"
  - "[[Graph Convolutional Network]]"
  - "[[Graph Attention Network]]"
related:
  - "[[Neural Networks]]"
  - "[[Kernel Methods]]"
status: complete
tags:
  - graph-learning-methods
---

# Graph Learning Methods

## Definition

Learn from data whose entities and relations form a graph.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

Message passing updates node states as $$h_v^{(l+1)}=U^{(l)}(h_v^{(l)},\operatorname{AGG}_{u\in\mathcal{N}(v)}M^{(l)}(h_v^{(l)},h_u^{(l)},e_{uv}))$$.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Graph Neural Network]]
- [[Graph Convolutional Network]]
- [[Graph Attention Network]]

## Family Tree

```text
Graph Learning Methods
├── Graph Neural Network
├── Graph Convolutional Network
└── Graph Attention Network
```

## Shared Strengths

Encodes relational inductive bias, permutation equivariance, and variable graph structure.

## Shared Limitations

Oversmoothing, oversquashing, sampling cost, graph leakage, sensitivity to missing edges, and limited expressive power for some structures.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Graph Neural Network]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Graph Attention Network]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Neural Networks]]
- [[Kernel Methods]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
