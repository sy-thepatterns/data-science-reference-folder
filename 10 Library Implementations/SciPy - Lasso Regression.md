---
type: implementation
name: SciPy - Lasso Regression
algorithm:
  - "[[Lasso Regression]]"
library:
  - "[[SciPy]]"
status: reviewed
tags:
  - lasso
  - implementation
---

# SciPy - Lasso Regression

## Implements

A custom construction; no dedicated estimator for [[Lasso Regression]], whose defining objective is squared residual loss plus an L1 coefficient penalty.

## Support Level

**Custom construction; no dedicated estimator.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
beta = soft_threshold(beta - step * X.T @ (X @ beta - y) / n, step * alpha)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `scipy sparse operations plus a custom proximal solver` |
| Fitting style | Proximal gradient, coordinate descent, or generic constrained reformulation |
| Core solver route | User-selected optimization routine |
| Statistical inference | None automatic |
| Sparse support | Yes for custom operations |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L1 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Proximal gradient, coordinate descent, or generic constrained reformulation
    ↓
User-selected optimization routine
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
\text{O(T nnz(X)) sparse high-level}
$$

Representative additional or active space:

$$
\text{O(nnz(X) + p)}
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

A generic smooth `minimize` call is not a faithful treatment of the nondifferentiable L1 kink unless the problem is reformulated or a suitable method is supplied.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Lasso Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official SciPy documentation for the named API or building blocks.
- [[Lasso Regression]]
- [[Lasso Regression Implementation Comparison]]

