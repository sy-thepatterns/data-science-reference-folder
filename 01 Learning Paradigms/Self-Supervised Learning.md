---
type: learning-paradigm
name: Self-Supervised Learning
tasks:
  - "[[Representation Learning]]"
  - "[[Generative Modelling]]"
algorithms:
  - "[[Contrastive Learning]]"
  - "[[Masked Language Modelling]]"
  - "[[Autoencoder]]"
architectures: []
related:
  - "[[Unsupervised Learning]]"
  - "[[Supervised Learning]]"
  - "[[Transfer Learning]]"
status: complete
tags:
  - representation-learning
---

# Self-Supervised Learning

## Definition

Self-supervised learning constructs prediction targets from the data itself and uses them to learn representations or generative models without manual labels.

## Learning Signal

Targets arise from held-out, transformed, masked, ordered, or paired parts of observations.

## Data Structure

Training data must be partitioned according to the unit that will generalize: examples, users, time, environments, or tasks. Any statistics learned during preprocessing belong inside the training split.

## Formal Setup

Sample a transformation or view mechanism $$t$$ and minimize a pretext loss $$\mathbb{E}_{x,t}[\ell(g_\theta(t(x)),\,s(x,t))]$$, where $$s$$ is generated from the observation rather than supplied by a human.

## Typical Objective

Contrastive, reconstruction, predictive, and redundancy-reduction objectives encode different invariances; none is synonymous with the paradigm.

## Main Tasks

- [[Representation Learning]]
- [[Generative Modelling]]

## Representative Algorithms

- [[Contrastive Learning]]
- [[Masked Language Modelling]]
- [[Autoencoder]]

## Evaluation

Use held-out units that reproduce deployment conditions. Compare against a simple baseline, report uncertainty across splits or seeds, and measure data, label, compute, and latency costs when relevant.

## Strengths

Uses abundant unlabelled data and can produce reusable representations.

## Limitations

Shortcut learning, representation collapse, compute cost, unwanted invariances, data contamination, and weak alignment with downstream goals.

## Related Paradigms

- [[Unsupervised Learning]]
- [[Supervised Learning]]
- [[Transfer Learning]]

## References

- Bishop, *Pattern Recognition and Machine Learning*, 2006.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
