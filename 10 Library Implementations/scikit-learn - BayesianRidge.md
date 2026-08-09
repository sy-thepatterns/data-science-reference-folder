---
type: implementation
name: scikit-learn - BayesianRidge
algorithm:
  - "[[Bayesian Linear Regression]]"
library:
  - "[[scikit-learn]]"
status: reviewed
tags:
  - bayesian
  - implementation
---

# scikit-learn - BayesianRidge

## Implements

A native empirical-bayes estimator for [[Bayesian Linear Regression]], whose defining objective is posterior and posterior-predictive inference for a linear Gaussian model.

## Support Level

**Native empirical-Bayes estimator.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
from sklearn.linear_model import BayesianRidge
model = BayesianRidge().fit(X, y)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `sklearn.linear_model.BayesianRidge` |
| Fitting style | Iterative evidence maximization for isotropic coefficient and noise precisions |
| Core solver route | MacKay/Tipping-style fixed-point updates with dense linear algebra |
| Statistical inference | Predictive standard deviation and coefficient covariance |
| Sparse support | Dense route |
| GPU support | No |

## Objective Mapping

The intended mathematical target is posterior and posterior-predictive inference for a linear Gaussian model. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Iterative evidence maximization for isotropic coefficient and noise precisions
    ↓
MacKay/Tipping-style fixed-point updates with dense linear algebra
    ↓
scikit-learn numerical operations and dependencies
    ↓
available CPU or accelerator backend
```

## Complexity Variables

$$
n=\text{number of samples}
$$

$$
p=\text{number of features}
$$

$$
T=\text{number of solver iterations or training passes}
$$

$$
b=\text{mini-batch size}
$$

## Training Complexity

Representative time:

$$
\text{Approximately O(T(np^2 + p^3)) in coefficient space}
$$

Representative additional or active space:

$$
\text{O(np + p^2)}
$$

These are route-level summaries, not universal bounds. Data shape, sparsity, active-set size, precision, convergence tolerance, line searches, batching, and linked numerical libraries can change actual cost.

## Prediction Complexity

For a dense fitted coefficient vector and:

$$
m=\text{number of prediction rows}
$$

prediction is dominated by a matrix-vector product:

$$
O(mp)
$$

with output storage:

$$
O(m)
$$

Sparse learned coefficients or sparse inputs can reduce arithmetic when the implementation exploits them.

## Numerical and Statistical Caveats

This is a specific Bayesian ridge model with hyperparameters estimated from data, not a general prior/likelihood programming interface.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Bayesian Linear Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official scikit-learn documentation for the named API or building blocks.
- [[Bayesian Linear Regression]]
- [[Bayesian Linear Regression Implementation Comparison]]

