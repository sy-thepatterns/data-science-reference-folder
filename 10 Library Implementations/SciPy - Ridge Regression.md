---
type: implementation
name: SciPy - Ridge Regression
algorithm:
  - "[[Ridge Regression]]"
library:
  - "[[SciPy]]"
status: reviewed
tags:
  - ridge
  - implementation
---

# SciPy - Ridge Regression

## Implements

A custom numerical construction for [[Ridge Regression]], whose defining objective is squared residual loss plus an L2 coefficient penalty.

## Support Level

**Custom numerical construction.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
from scipy.linalg import solve
G = X.T @ X + alpha * np.eye(X.shape[1])
beta = solve(G, X.T @ y, assume_a="pos")
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `scipy.linalg.solve, cho_factor/cho_solve, or scipy.sparse.linalg` |
| Fitting style | Direct dense or iterative sparse solve |
| Core solver route | LAPACK or sparse iterative method |
| Statistical inference | None automatic |
| Sparse support | Yes through scipy.sparse |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L2 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Direct dense or iterative sparse solve
    ↓
LAPACK or sparse iterative method
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
\text{Dense O(np^2 + p^3); sparse iterative O(T nnz(X))}
$$

Representative additional or active space:

$$
\text{Dense O(np + p^2); sparse route-dependent}
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

`assume_a="pos"` is valid only when the penalized system is positive definite; augmented QR/SVD avoids squaring conditioning.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Ridge Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official SciPy documentation for the named API or building blocks.
- [[Ridge Regression]]
- [[Ridge Regression Implementation Comparison]]

