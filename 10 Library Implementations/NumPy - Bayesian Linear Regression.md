---
type: implementation
name: NumPy - Bayesian Linear Regression
algorithm:
  - "[[Bayesian Linear Regression]]"
library:
  - "[[NumPy]]"
status: reviewed
tags:
  - bayesian
  - implementation
---

# NumPy - Bayesian Linear Regression

## Implements

A custom exact conjugate implementation for [[Bayesian Linear Regression]], whose defining objective is posterior and posterior-predictive inference for a linear Gaussian model.

## Public API

```python
precision_n = precision_0 + X.T @ X / sigma2
mean_n = np.linalg.solve(precision_n, rhs)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `numpy.linalg solve/cholesky plus random generation` |
| Fitting style | Posterior precision and natural-parameter updates |
| Core solver route | Linked LAPACK routines |
| Statistical inference | Manual but exact under conjugacy |
| Sparse support | No in numpy.linalg |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is posterior and posterior-predictive inference for a linear Gaussian model. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Posterior precision and natural-parameter updates
    ↓
Linked LAPACK routines
    ↓
NumPy numerical operations and dependencies
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
\text{O(np^2 + p^3)}
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

Avoid explicit matrix inversion; posterior correctness depends on the exact prior and known/unknown noise model.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Bayesian Linear Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official NumPy documentation for the named API or building blocks.
- [[Bayesian Linear Regression]]
- [[Bayesian Linear Regression Implementation Comparison]]
