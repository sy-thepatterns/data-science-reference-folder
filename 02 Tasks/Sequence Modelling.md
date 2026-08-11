---
type: task
name: Sequence Modelling
learning_paradigms:
  - "[[Unsupervised Learning]]"
algorithm_families:
  - "[[Neural Networks]]"
  - "[[Probabilistic Models]]"
algorithms: []
metrics:
  - "[[Negative Log-Likelihood]]"
  - "[[Perplexity]]"
  - "[[Edit Distance]]"
datasets: []
applications:
  - "[[Language]]"
  - "[[Speech]]"
  - "[[Biological sequences]]"
status: complete
tags:
  - sequence-modelling
---

# Sequence Modelling

## Problem Definition

Model ordered observations whose position and dependence structure carry information.

## Inputs

variable-length sequences $$(x_1,\ldots,x_T)$$ with masks and optional targets.

## Outputs

sequence labels, per-step labels, next-token distributions, or generated sequences.

## Formal Setup

Autoregressive models factorize $$p(x_{1:T})=\prod_{t=1}^{T}p(x_t\mid x_{<t})$$; other factorizations support different inference patterns.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Neural Networks]]
- [[Probabilistic Models]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Negative Log-Likelihood]]
- [[Perplexity]]
- [[Edit Distance]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Language]]
- [[Speech]]
- [[Biological sequences]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Time-Series Modelling]]
- [[Forecasting]]
- [[Representation Learning]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
