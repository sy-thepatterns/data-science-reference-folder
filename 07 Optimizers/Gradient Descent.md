---
type: optimizer
name: Gradient Descent
status: stub
tags: []
---

# Gradient Descent

## Notation

| Symbol | Meaning |
|---|---|
| $\theta_t$ | Parameter vector after step $t$. |
| $t$ | Iteration number. |
| $L(\theta)$ | Objective or loss being minimized. |
| $\nabla_\theta L(\theta_t)$ | Gradient: vector of local slopes of $L$ at $\theta_t$. |
| $\eta_t$ | Positive learning rate, or step size, used at iteration $t$. |
| $T$ | Total number of optimization steps. |

## Intuition

Imagine standing on a foggy hillside and wanting to reach a low point. The gradient points in the steepest uphill direction, so gradient descent takes a small step the opposite way. The learning rate controls whether that step is cautious, useful, or so large that you jump across the valley.

## Update Rule

$$
\theta_{t+1}
=
\theta_t-\eta_t\nabla_\theta L(\theta_t)
$$

## Derivation or Proof

These are useful routes for understanding the update and its guarantees:

- Use the first-order Taylor approximation to show that a sufficiently small step along the negative gradient decreases a differentiable objective.
- Use the descent lemma to derive an explicit safe step-size range when the gradient is Lipschitz continuous.
- For a convex objective, combine convexity with the update identity to prove convergence-rate bounds; strong convexity gives faster rates under stronger assumptions.

## Summary

An iterative method that moves opposite the objective gradient.

## Formal Definition

## Complexity

## Related Notes

## References

1. [Ruder — “An Overview of Gradient Descent Optimization Algorithms”](https://arxiv.org/abs/1609.04747). Open survey — batch, stochastic, momentum, and adaptive gradient methods.
2. [Boyd and Vandenberghe — *Convex Optimization*](https://web.stanford.edu/~boyd/cvxbook/). Open textbook — first-order methods, convexity, convergence, and step sizes.
3. [Zhang et al. — *Dive into Deep Learning*](https://d2l.ai/). Open textbook — neural networks, optimization, sequence models, attention, computer vision, and implementation examples.
4. [Murphy — *Probabilistic Machine Learning: An Introduction*](https://probml.github.io/pml-book/book1.html). Open textbook — probability, statistics, supervised learning, optimization, linear algebra, and probabilistic modelling.
