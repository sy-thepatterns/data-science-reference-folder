---
type: implementation
name: NumPy - Lasso Regression
algorithm:
  - "[[Lasso Regression]]"
library:
  - "[[NumPy]]"
status: reviewed
tags:
  - lasso
  - implementation
---

# NumPy - Lasso Regression

## Implements

A custom construction; no native estimator for [[Lasso Regression]], whose defining objective is squared residual loss plus an L1 coefficient penalty.

## Support Level

**Custom construction; no native estimator.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
beta = soft_threshold(beta - step * grad, step * alpha)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `Array operations in a custom coordinate/proximal solver` |
| Fitting style | Coordinate descent or proximal gradient |
| Core solver route | User-authored numerical algorithm |
| Statistical inference | None automatic |
| Sparse support | Manual |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L1 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Coordinate descent or proximal gradient
    ↓
User-authored numerical algorithm
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
\text{O(Tnp) dense}
$$

Representative additional or active space:

$$
\text{O(np + p)}
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

`numpy.linalg.lstsq` does not solve lasso; convergence requires a valid step size or correct coordinate updates and a stopping diagnostic.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Lasso Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official NumPy documentation for the named API or building blocks.
- [[Lasso Regression]]
- [[Lasso Regression Implementation Comparison]]

