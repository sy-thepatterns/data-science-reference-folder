---
type: algorithm-family
name: Neural Networks
parent_family: []
tasks: []
members:
  - "[[Multilayer Perceptron]]"
  - "[[Convolutional Neural Network]]"
  - "[[Recurrent Neural Network]]"
  - "[[Transformer]]"
  - "[[Graph Neural Network]]"
related:
  - "[[Generative Models]]"
  - "[[Graph Learning Methods]]"
  - "[[Linear Models]]"
status: complete
tags:
  - neural-networks
---

# Neural Networks

## Definition

Compose parameterized affine transformations and nonlinear operations into differentiable computation graphs.

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

A feed-forward layer computes $h^{(l+1)}=\sigma(W^{(l)}h^{(l)}+b^{(l)})$; architectures impose structure through connectivity, sharing, attention, recurrence, or equivariance.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Multilayer Perceptron]]
- [[Convolutional Neural Network]]
- [[Recurrent Neural Network]]
- [[Transformer]]
- [[Graph Neural Network]]

## Family Tree

```text
Neural Networks
├── Multilayer Perceptron
├── Convolutional Neural Network
└── Graph Neural Network
```

## Shared Strengths

Highly expressive, representation learning, scalable minibatch optimization, and support for multimodal structured inputs.

## Shared Limitations

Data and compute intensity, nonconvex training, calibration and robustness issues, difficult attribution, and strong sensitivity to optimization and preprocessing.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Multilayer Perceptron]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Graph Neural Network]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Generative Models]]
- [[Graph Learning Methods]]
- [[Linear Models]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
