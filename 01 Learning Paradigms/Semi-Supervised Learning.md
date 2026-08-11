---
type: learning-paradigm
name: Semi-Supervised Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
algorithms:
  - "[[Pseudo-Labeling]]"
  - "[[Consistency Regularization]]"
  - "[[Label Propagation]]"
architectures: []
related:
  - "[[Supervised Learning]]"
  - "[[Unsupervised Learning]]"
  - "[[Self-Supervised Learning]]"
status: complete
tags:
  - semi-supervised
---

# Semi-Supervised Learning

## Definition

Semi-supervised learning combines a small labelled set with a larger unlabelled set to improve prediction.

## Learning Signal

Direct targets for labelled examples plus structural or consistency constraints derived from unlabelled examples.

## Data Structure

Training data must be partitioned according to the unit that will generalize: examples, users, time, environments, or tasks. Any statistics learned during preprocessing belong inside the training split.

## Formal Setup

With labelled $$\mathcal{L}$$ and unlabelled $$\mathcal{U}$$, optimize $$L(\theta)=L_{\mathrm{sup}}(\theta;\mathcal{L})+\lambda L_{\mathrm{unsup}}(\theta;\mathcal{U})$$.

## Typical Objective

The unsupervised term may enforce consistency, entropy minimization, graph smoothness, or agreement with pseudo-labels.

## Main Tasks

- [[Classification]]
- [[Regression]]

## Representative Algorithms

- [[Pseudo-Labeling]]
- [[Consistency Regularization]]
- [[Label Propagation]]

## Evaluation

Use held-out units that reproduce deployment conditions. Compare against a simple baseline, report uncertainty across splits or seeds, and measure data, label, compute, and latency costs when relevant.

## Strengths

Can improve accuracy when labels are scarce and unlabelled data match the target distribution.

## Limitations

Confirmation bias, class mismatch, distribution shift, poorly calibrated pseudo-labels, and invalid smoothness or cluster assumptions.

## Related Paradigms

- [[Supervised Learning]]
- [[Unsupervised Learning]]
- [[Self-Supervised Learning]]

## References

- Bishop, *Pattern Recognition and Machine Learning*, 2006.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
