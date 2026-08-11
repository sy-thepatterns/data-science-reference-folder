---
type: task
name: Dimensionality Reduction
learning_paradigms:
  - "[[Unsupervised Learning]]"
algorithm_families:
  - "[[Linear Models]]"
  - "[[Kernel Methods]]"
  - "[[Neural Networks]]"
  - "[[Graph Learning Methods]]"
algorithms: []
metrics:
  - "[[Reconstruction Error]]"
  - "[[Trustworthiness]]"
  - "[[Explained Variance Ratio]]"
datasets: []
applications:
  - "[[Visualization]]"
  - "[[Compression]]"
  - "[[Preprocessing]]"
status: complete
tags:
  - dimensionality-reduction
---

# Dimensionality Reduction

## Problem Definition

Map data to fewer coordinates while preserving information specified by a reconstruction, variance, distance, neighbourhood, or task-aware criterion.

## Inputs

matrix $$X\in\mathbb{R}^{n\times p}$$ or pairwise relations.

## Outputs

embedding $$Z\in\mathbb{R}^{n\times d}$$ with $$d<p$$, often with an inverse map.

## Formal Setup

Learn $$f:\mathbb{R}^p\to\mathbb{R}^d$$ and possibly $$g$$ to minimize a criterion such as $$\sum_i\lVert x_i-g(f(x_i))\rVert_2^2$$.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Linear Models]]
- [[Kernel Methods]]
- [[Neural Networks]]
- [[Graph Learning Methods]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Reconstruction Error]]
- [[Trustworthiness]]
- [[Explained Variance Ratio]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Visualization]]
- [[Compression]]
- [[Preprocessing]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Representation Learning]]
- [[Clustering]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
