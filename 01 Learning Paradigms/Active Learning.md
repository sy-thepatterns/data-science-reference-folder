---
type: learning-paradigm
name: Active Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
algorithms:
  - "[[Uncertainty Sampling]]"
  - "[[Query by Committee]]"
architectures: []
related:
  - "[[Supervised Learning]]"
  - "[[Online Learning]]"
  - "[[Semi-Supervised Learning]]"
status: complete
tags:
  - supervised
  - incremental
---

# Active Learning

## Definition

Active learning is supervised learning in which the learner chooses which unlabelled examples should be labelled, usually because labels are costly.

## Learning Signal

Labels supplied by an oracle in response to queries. Pool-based, stream-based, and membership-query settings differ in which examples may be queried.

## Data Structure

Training data must be partitioned according to the unit that will generalize: examples, users, time, environments, or tasks. Any statistics learned during preprocessing belong inside the training split.

## Formal Setup

Given labelled set $$\mathcal{L}$$, unlabelled pool $$\mathcal{U}$$, model $$f_\theta$$, and budget $$B$$, choose a query policy $$q$$ to minimize final generalization risk after at most $$B$$ oracle calls.

## Typical Objective

A common proxy selects the point with maximum predictive uncertainty: $$x^{\star}=\arg\max_{x\in\mathcal{U}} H[p_\theta(y\mid x)]$$. The acquisition rule is not the predictive model or its optimizer.

## Main Tasks

- [[Classification]]
- [[Regression]]

## Representative Algorithms

- [[Uncertainty Sampling]]
- [[Query by Committee]]

## Evaluation

Use held-out units that reproduce deployment conditions. Compare against a simple baseline, report uncertainty across splits or seeds, and measure data, label, compute, and latency costs when relevant.

## Strengths

Can reduce annotation cost and focus expert effort on informative cases.

## Limitations

Selection bias, noisy or delayed oracles, poor uncertainty estimates, nonrepresentative pools, and batch redundancy can erase gains.

## Related Paradigms

- [[Supervised Learning]]
- [[Online Learning]]
- [[Semi-Supervised Learning]]

## References

- Bishop, *Pattern Recognition and Machine Learning*, 2006.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
