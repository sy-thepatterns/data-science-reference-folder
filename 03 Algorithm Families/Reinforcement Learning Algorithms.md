---
type: algorithm-family
name: Reinforcement Learning Algorithms
parent_family: []
tasks: []
members:
  - "[[Q-Learning]]"
  - "[[Policy Gradient]]"
  - "[[Actor-Critic]]"
  - "[[Model-Based Reinforcement Learning]]"
related:
  - "[[Neural Networks]]"
  - "[[Probabilistic Models]]"
status: complete
tags:
  - reinforcement-learning-algorithms
---

# Reinforcement Learning Algorithms

## Definition

Estimate policies, value functions, or environment models for sequential decision problems.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

The Bellman optimality relation is $$Q^{\star}(s,a)=\mathbb{E}[r+\gamma\max_{a'}Q^{\star}(s',a')\mid s,a]$$; policy methods instead optimize expected return directly.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Q-Learning]]
- [[Policy Gradient]]
- [[Actor-Critic]]
- [[Model-Based Reinforcement Learning]]

## Family Tree

```text
Reinforcement Learning Algorithms
├── Q-Learning
├── Policy Gradient
└── Model-Based Reinforcement Learning
```

## Shared Strengths

Addresses delayed consequences, exploration, and adaptive policies.

## Shared Limitations

High variance, sample inefficiency, instability with function approximation, unsafe exploration, reward gaming, and difficult counterfactual evaluation.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Q-Learning]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Model-Based Reinforcement Learning]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Neural Networks]]
- [[Probabilistic Models]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
