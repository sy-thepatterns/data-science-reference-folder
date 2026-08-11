---
type: learning-paradigm
name: Meta-Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
  - "[[Representation Learning]]"
algorithms:
  - "[[Model-Agnostic Meta-Learning]]"
  - "[[Prototypical Networks]]"
architectures: []
related:
  - "[[Transfer Learning]]"
  - "[[Supervised Learning]]"
status: complete
tags:
  - transfer-learning
---

# Meta-Learning

## Definition

Meta-learning learns across a distribution of tasks so that a new task can be learned or adapted with little data or computation.

## Learning Signal

A collection of tasks, each normally split into a support set for adaptation and a query set for measuring post-adaptation performance.

## Data Structure

Training data must be partitioned according to the unit that will generalize: examples, users, time, environments, or tasks. Any statistics learned during preprocessing belong inside the training split.

## Formal Setup

For tasks $$\tau\sim p(\tau)$$ with adaptation operator $$A$$, learn meta-parameters $$\phi$$ that perform well after adaptation: $$\phi^{\star}=\arg\min_\phi\mathbb{E}_\tau[L_{\tau}^{\mathrm{query}}(A(\phi,\mathcal{D}_{\tau}^{\mathrm{support}}))]$$.

## Typical Objective

Objectives vary: gradient-based rapid adaptation, learned optimizers, metric learning, or amortized inference. These are distinct algorithm families under the paradigm.

## Main Tasks

- [[Classification]]
- [[Regression]]
- [[Representation Learning]]

## Representative Algorithms

- [[Model-Agnostic Meta-Learning]]
- [[Prototypical Networks]]

## Evaluation

Use held-out units that reproduce deployment conditions. Compare against a simple baseline, report uncertainty across splits or seeds, and measure data, label, compute, and latency costs when relevant.

## Strengths

Shares statistical strength across related tasks and supports few-shot adaptation.

## Limitations

Task-distribution mismatch, expensive nested optimization, meta-overfitting, and ambiguous train/test task boundaries.

## Related Paradigms

- [[Transfer Learning]]
- [[Supervised Learning]]

## References

- Bishop, *Pattern Recognition and Machine Learning*, 2006.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
