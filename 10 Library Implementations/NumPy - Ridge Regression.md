---
type: implementation
name: NumPy - Ridge Regression
algorithm:
  - "[[Ridge Regression]]"
library:
  - "[[NumPy]]"
status: reviewed
tags:
  - ridge
  - implementation
---

# NumPy - Ridge Regression

## Implements

A custom construction for [[Ridge Regression]], whose defining objective is squared residual loss plus an L2 coefficient penalty.

## Public API

```python
G = X.T @ X + alpha * np.eye(X.shape[1])
beta = np.linalg.solve(G, X.T @ y)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `numpy.linalg.solve or numpy.linalg.lstsq` |
| Fitting style | Direct linear-system or augmented least-squares solve |
| Core solver route | Linked LAPACK routine |
| Statistical inference | None automatic |
| Sparse support | No in numpy.linalg |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L2 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Direct linear-system or augmented least-squares solve
    ↓
Linked LAPACK routine
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
\text{O(np^2 + p^3) in coefficient space}
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

Do not penalize an intercept column unintentionally; an augmented least-squares formulation is often more stable than forming the Gram matrix.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Ridge Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official NumPy documentation for the named API or building blocks.
- [[Ridge Regression]]
- [[Ridge Regression Implementation Comparison]]
