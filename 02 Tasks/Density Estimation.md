---
type: task
name: Density Estimation
learning_paradigms:
  - "[[Unsupervised Learning]]"
algorithm_families:
  - "[[Probabilistic Models]]"
  - "[[Kernel Methods]]"
  - "[[Generative Models]]"
algorithms: []
metrics:
  - "[[Negative Log-Likelihood]]"
  - "[[Brier Score]]"
datasets: []
applications:
  - "[[Simulation]]"
  - "[[Uncertainty modelling]]"
  - "[[Anomaly scoring]]"
status: complete
tags:
  - density-estimation
---

# Density Estimation

## Problem Definition

Estimate a probability mass or density function for observed data, conditionally or unconditionally.

## Inputs

samples $$x_i$$, or pairs $$(x_i,y_i)$$ for conditional density estimation.

## Outputs

a normalized density, mass function, likelihood, or samples from the fitted distribution.

## Formal Setup

Maximum likelihood solves $$\hat\theta=\arg\max_\theta\sum_i\log p_\theta(x_i)$$; nonparametric estimators may not use finite-dimensional $$\theta$$.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Probabilistic Models]]
- [[Kernel Methods]]
- [[Generative Models]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Negative Log-Likelihood]]
- [[Brier Score]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Simulation]]
- [[Uncertainty modelling]]
- [[Anomaly scoring]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Generative Modelling]]
- [[Anomaly Detection]]
- [[Regression]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
