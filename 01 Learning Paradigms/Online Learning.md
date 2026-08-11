---
type: learning-paradigm
name: Online Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
  - "[[Forecasting]]"
  - "[[Recommendation]]"
algorithms:
  - "[[Online Gradient Descent]]"
  - "[[Follow the Regularized Leader]]"
architectures: []
related:
  - "[[Supervised Learning]]"
  - "[[Active Learning]]"
  - "[[Reinforcement Learning]]"
status: complete
tags:
  - online
  - incremental
---

# Online Learning

## Definition

Online learning updates a predictor sequentially as examples or feedback arrive, often under memory or latency constraints.

## Learning Signal

At round $$t$$, the learner predicts before observing the outcome or loss, then receives feedback and updates.

## Data Structure

Training data must be partitioned according to the unit that will generalize: examples, users, time, environments, or tasks. Any statistics learned during preprocessing belong inside the training split.

## Formal Setup

For rounds $$t=1,\ldots,T$$, choose $$w_t$$, incur loss $$\ell_t(w_t)$$, and compare cumulative loss with a comparator $$u$$ through regret: $$R_T(u)=\sum_{t=1}^{T}\ell_t(w_t)-\sum_{t=1}^{T}\ell_t(u)$$.

## Typical Objective

Typical guarantees bound static or dynamic regret. An update such as $$w_{t+1}=w_t-\eta_t\nabla\ell_t(w_t)$$ is an optimizer, not the paradigm itself.

## Main Tasks

- [[Classification]]
- [[Regression]]
- [[Forecasting]]
- [[Recommendation]]

## Representative Algorithms

- [[Online Gradient Descent]]
- [[Follow the Regularized Leader]]

## Evaluation

Use held-out units that reproduce deployment conditions. Compare against a simple baseline, report uncertainty across splits or seeds, and measure data, label, compute, and latency costs when relevant.

## Strengths

Adapts continuously, uses bounded memory, and can respond to drift.

## Limitations

Order sensitivity, delayed feedback, catastrophic adaptation, concept drift, and tuning stability–plasticity trade-offs.

## Related Paradigms

- [[Supervised Learning]]
- [[Active Learning]]
- [[Reinforcement Learning]]

## References

- Bishop, *Pattern Recognition and Machine Learning*, 2006.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
