---
type: task
name: Forecasting
learning_paradigms:
  - "[[Supervised Learning]]"
algorithm_families:
  - "[[Linear Models]]"
  - "[[Probabilistic Models]]"
  - "[[Neural Networks]]"
  - "[[Ensemble Methods]]"
algorithms: []
metrics:
  - "[[Mean Absolute Error]]"
  - "[[Root Mean Squared Error]]"
  - "[[Pinball Loss]]"
datasets: []
applications:
  - "[[Demand planning]]"
  - "[[Energy load]]"
  - "[[Capacity planning]]"
status: complete
tags:
  - forecasting
---

# Forecasting

## Problem Definition

Predict values or distributions at future horizons using information available at a forecast origin.

## Inputs

time-indexed history, covariates known at prediction time, and a forecast horizon.

## Outputs

point, quantile, interval, or probabilistic forecasts for future times.

## Formal Setup

At origin $$t$$ and horizon $$h$$, estimate $$p(y_{t+h}\mid\mathcal{F}_t)$$ without using information arriving after $$t$$.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Linear Models]]
- [[Probabilistic Models]]
- [[Neural Networks]]
- [[Ensemble Methods]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Mean Absolute Error]]
- [[Root Mean Squared Error]]
- [[Pinball Loss]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Demand planning]]
- [[Energy load]]
- [[Capacity planning]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Regression]]
- [[Time-Series Modelling]]
- [[Sequence Modelling]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
