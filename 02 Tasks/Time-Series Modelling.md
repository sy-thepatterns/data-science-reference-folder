---
type: task
name: Time-Series Modelling
learning_paradigms:
  - "[[Unsupervised Learning]]"
algorithm_families:
  - "[[Probabilistic Models]]"
  - "[[Linear Models]]"
  - "[[Neural Networks]]"
algorithms: []
metrics:
  - "[[Log-Likelihood]]"
  - "[[Autocorrelation Diagnostics]]"
  - "[[Root Mean Squared Error]]"
datasets: []
applications:
  - "[[Monitoring]]"
  - "[[Econometrics]]"
  - "[[Sensor analysis]]"
status: complete
tags:
  - time-series-modelling
---

# Time-Series Modelling

## Problem Definition

Model observations indexed by time while respecting temporal dependence, sampling, trend, seasonality, and changing distributions.

## Inputs

one or more time series with timestamps, covariates, missingness, and sampling metadata.

## Outputs

a fitted temporal process, latent states, decompositions, or forecasts.

## Formal Setup

Represent $$p(y_{1:T}\mid x_{1:T})$$ or structural components while ensuring estimates at time $$t$$ only use information permitted at that time.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Probabilistic Models]]
- [[Linear Models]]
- [[Neural Networks]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Log-Likelihood]]
- [[Autocorrelation Diagnostics]]
- [[Root Mean Squared Error]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Monitoring]]
- [[Econometrics]]
- [[Sensor analysis]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Forecasting]]
- [[Sequence Modelling]]
- [[Anomaly Detection]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
