---
type: learning-paradigm
name: Unsupervised Learning
tasks:
  - "[[Clustering]]"
  - "[[Density Estimation]]"
  - "[[Dimensionality Reduction]]"
  - "[[Anomaly Detection]]"
  - "[[Representation Learning]]"
algorithms:
  - "[[k-Means]]"
  - "[[Gaussian Mixture Model]]"
  - "[[Principal Component Analysis]]"
architectures: []
related:
  - "[[Self-Supervised Learning]]"
  - "[[Semi-Supervised Learning]]"
  - "[[Generative Modelling]]"
status: complete
tags:
  - unsupervised
---

# Unsupervised Learning

## Definition

Unsupervised learning seeks structure in observations without externally supplied target labels.

## Learning Signal

The empirical distribution, geometry, dependence, or augmentations of the observed inputs.

## Data Structure

Training data must be partitioned according to the unit that will generalize: examples, users, time, environments, or tasks. Any statistics learned during preprocessing belong inside the training split.

## Formal Setup

Given $$\mathcal{D}=\{x_i\}_{i=1}^{n}$$, fit a representation, partition, density, or latent-variable model according to an explicit objective and inductive assumptions.

## Typical Objective

Likelihood, reconstruction, distortion, contrastive, and manifold objectives define different estimands and should not be treated as interchangeable.

## Main Tasks

- [[Clustering]]
- [[Density Estimation]]
- [[Dimensionality Reduction]]
- [[Anomaly Detection]]
- [[Representation Learning]]

## Representative Algorithms

- [[k-Means]]
- [[Gaussian Mixture Model]]
- [[Principal Component Analysis]]

## Evaluation

Use held-out units that reproduce deployment conditions. Compare against a simple baseline, report uncertainty across splits or seeds, and measure data, label, compute, and latency costs when relevant.

## Strengths

Uses unlabelled data and supports exploration, compression, and pretraining.

## Limitations

Objectives may not match semantic usefulness; evaluation is indirect and solutions can be nonidentifiable or unstable.

## Related Paradigms

- [[Self-Supervised Learning]]
- [[Semi-Supervised Learning]]
- [[Generative Modelling]]

## References

- Bishop, *Pattern Recognition and Machine Learning*, 2006.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
