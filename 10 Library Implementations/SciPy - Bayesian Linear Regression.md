---
type: implementation
name: SciPy - Bayesian Linear Regression
algorithm:
  - "[[Bayesian Linear Regression]]"
library:
  - "[[SciPy]]"
status: reviewed
tags:
  - bayesian
  - implementation
---

# SciPy - Bayesian Linear Regression

## Implements

A custom exact conjugate implementation for [[Bayesian Linear Regression]], whose defining objective is posterior and posterior-predictive inference for a linear Gaussian model.

## Public API

```python
c, lower = scipy.linalg.cho_factor(precision_n)
mean_n = scipy.linalg.cho_solve((c, lower), rhs)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `scipy.linalg plus scipy.stats distributions` |
| Fitting style | Stable factorizations and probability-distribution utilities |
| Core solver route | Cholesky/triangular solves or other LAPACK routes |
| Statistical inference | Manual posterior and predictive distributions |
| Sparse support | Possible with sparse solvers |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is posterior and posterior-predictive inference for a linear Gaussian model. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Stable factorizations and probability-distribution utilities
    ↓
Cholesky/triangular solves or other LAPACK routes
    ↓
SciPy numerical operations and dependencies
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
\text{Dense O(np^2 + p^3)}
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

SciPy provides components, not a unified estimator; distribution parameterizations and factor orientation must be checked carefully.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Bayesian Linear Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official SciPy documentation for the named API or building blocks.
- [[Bayesian Linear Regression]]
- [[Bayesian Linear Regression Implementation Comparison]]
