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
algorithms:
  - "[[Linear Regression]]"
  - "[[Ridge Regression]]"
  - "[[Lasso Regression]]"
  - "[[Elastic Net]]"
  - "[[Huber Regression]]"
  - "[[Bayesian Linear Regression]]"
metrics:
  - "[[Mean Squared Error]]"
  - "[[Mean Absolute Error]]"
  - "[[Root Mean Squared Error]]"
  - "[[R Squared]]"
applications:
  - "[[Continuous Outcome Prediction]]"
status: reviewed
tags:
  - regression
---

# Regression

## Problem Definition

Regression predicts a continuous or quantitatively ordered response from one or more inputs. The task definition does not prescribe a model family, loss function, optimizer, solver, software library, or hardware backend.

## Inputs

A dataset of input-output pairs:

$$
\mathcal{\{D\}}
=
\{\,(x_i,y_i)\,\}_{i=1}^{n}
$$

with:

$$
x_i\in\mathcal{X}
$$

and commonly:

$$
y_i\in\mathbb{R}
$$

Targets may also be positive, bounded, counts, rates, or vectors. Their domain should influence model and likelihood choice.

## Outputs

A point predictor:

$$
\hat{f}:\mathcal{X}\rightarrow\mathbb{R}
$$

or a predictive distribution:

$$
\hat{p}(y\mid x)
$$

Uncertainty intervals, quantiles, or decisions may be derived when the model and evaluation design support them.

## Formal Setup

Under squared-error risk, the population-optimal point prediction is the conditional mean:

$$
f^{\star}(x)
=
\mathbb{E}[Y\mid X=x]
$$

Under absolute-error risk, it is a conditional median. Therefore, the loss function helps define the estimand rather than merely scoring an already fixed target.

## Algorithm Families

- [[Linear Models]]
- [[Tree-Based Methods]]
- [[Kernel Methods]]
- [[Neural Networks]]
- [[Bayesian Methods]]

## Representative Algorithms

- [[Linear Regression]]
- [[Ridge Regression]]
- [[Lasso Regression]]
- [[Elastic Net]]
- [[Huber Regression]]
- [[Bayesian Linear Regression]]

## Evaluation Metrics

- [[Mean Squared Error]] emphasizes large errors.
- [[Root Mean Squared Error]] returns to the target's units.
- [[Mean Absolute Error]] is less dominated by large residuals.
- [[R Squared]] measures improvement relative to a mean baseline under its usual definition.

Metrics should be chosen before inspecting test performance and should match deployment costs. Time, group, and entity structure must be respected during splitting.

## Common Failure Modes

- Leakage from preprocessing or target-derived features.
- Extrapolation beyond observed support.
- Distribution shift.
- Inappropriate loss for the target domain or decision cost.
- Reporting average error without subgroup or tail behaviour.
- Confusing predictive association with causal effect.
- Ignoring uncertainty when decisions are sensitive to it.

## Applications

- [[Continuous Outcome Prediction]]
- Forecasting numeric demand or measurements.
- Estimating risk, duration, cost, or intensity.

## Related Tasks

- [[Classification]]
- [[Forecasting]]
- [[Time-Series Modelling]]
- [[Ranking]]


