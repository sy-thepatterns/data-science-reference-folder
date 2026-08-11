---
type: task
name: Regression
learning_paradigms:
  - "[[Supervised Learning]]"
algorithm_families:
  - "[[Linear Models]]"
  - "[[Tree-Based Methods]]"
  - "[[Kernel Methods]]"
  - "[[Neural Networks]]"
  - "[[Bayesian Methods]]"
algorithms: []
metrics:
  - "[[Mean Squared Error]]"
  - "[[Mean Absolute Error]]"
  - "[[Root Mean Squared Error]]"
  - "[[R Squared]]"
datasets: []
applications:
  - "[[Continuous Outcome Prediction]]"
  - "[[Risk estimation]]"
  - "[[Duration prediction]]"
status: complete
tags:
  - regression
---

# Regression

## Problem Definition

Predict a continuous or quantitatively ordered response; the task does not prescribe a model, loss, solver, library, or backend.

## Inputs

pairs $$(x_i,y_i)$$, commonly with $$y_i\in\mathbb{R}$$.

## Outputs

point predictions, quantiles, intervals, or predictive distributions.

## Formal Setup

Under squared-error risk, $$f^{\star}(x)=\mathbb{E}[Y\mid X=x]$$; under absolute error it is a conditional median.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Linear Models]]
- [[Tree-Based Methods]]
- [[Kernel Methods]]
- [[Neural Networks]]
- [[Bayesian Methods]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Mean Squared Error]]
- [[Mean Absolute Error]]
- [[Root Mean Squared Error]]
- [[R Squared]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Continuous Outcome Prediction]]
- [[Risk estimation]]
- [[Duration prediction]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Classification]]
- [[Forecasting]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
