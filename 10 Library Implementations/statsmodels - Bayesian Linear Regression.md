---
type: implementation
name: statsmodels - Bayesian Linear Regression
algorithm:
  - "[[Bayesian Linear Regression]]"
library:
  - "[[statsmodels]]"
status: reviewed
tags:
  - bayesian
  - implementation
---

# statsmodels - Bayesian Linear Regression

## Implements

A custom conjugate construction using statsmodels-adjacent arrays/results for [[Bayesian Linear Regression]], whose defining objective is posterior and posterior-predictive inference for a linear Gaussian model.

## Public API

```python
# No direct statsmodels BayesianLinearRegression class
# implement conjugate updates or use a dedicated Bayesian package
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `No general core Bayesian linear-regression estimator` |
| Fitting style | User-authored posterior algebra |
| Core solver route | NumPy/SciPy linear algebra |
| Statistical inference | Manual posterior and predictive summaries |
| Sparse support | Limited |
| GPU support | No |

## Objective Mapping

The intended mathematical target is posterior and posterior-predictive inference for a linear Gaussian model. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
User-authored posterior algebra
    ↓
NumPy/SciPy linear algebra
    ↓
statsmodels numerical operations and dependencies
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
\text{Dense conjugate O(np^2 + p^3)}
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

statsmodels Bayesian mixed-model classes do not substitute for a general Bayesian linear-regression API; PyMC is a separate package.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Bayesian Linear Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official statsmodels documentation for the named API or building blocks.
- [[Bayesian Linear Regression]]
- [[Bayesian Linear Regression Implementation Comparison]]
