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

Message passing updates node states as $h_v^{(l+1)}=U^{(l)}(h_v^{(l)},\operatorname{AGG}_{u\in\mathcal{N}(v)}M^{(l)}(h_v^{(l)},h_u^{(l)},e_{uv}))$.

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
