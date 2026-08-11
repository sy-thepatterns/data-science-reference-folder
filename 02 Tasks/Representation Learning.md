---
type: task
name: Representation Learning
learning_paradigms:
  - "[[Unsupervised Learning]]"
algorithm_families:
  - "[[Neural Networks]]"
  - "[[Generative Models]]"
  - "[[Graph Learning Methods]]"
  - "[[Kernel Methods]]"
algorithms: []
metrics:
  - "[[Linear Probe Accuracy]]"
  - "[[Retrieval Recall]]"
  - "[[Reconstruction Error]]"
datasets: []
applications:
  - "[[Transfer learning]]"
  - "[[Retrieval]]"
  - "[[Visualization]]"
status: complete
tags:
  - representation-learning
---

# Representation Learning

## Problem Definition

Learn features or embeddings from data so that useful factors are easier to model downstream.

## Inputs

raw or partially processed observations and optional labels, pairs, views, or sequences.

## Outputs

embedding $$z=f_\theta(x)\in\mathbb{R}^d$$ or a distribution over latent variables.

## Formal Setup

Optimize a supervised, contrastive, reconstruction, predictive, or generative objective that encodes desired invariances and retained information.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Neural Networks]]
- [[Generative Models]]
- [[Graph Learning Methods]]
- [[Kernel Methods]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Linear Probe Accuracy]]
- [[Retrieval Recall]]
- [[Reconstruction Error]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Transfer learning]]
- [[Retrieval]]
- [[Visualization]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Dimensionality Reduction]]
- [[Generative Modelling]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
