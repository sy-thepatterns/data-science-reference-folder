---
type: implementation
name: scikit-learn - Ridge
algorithm:
  - "[[Ridge Regression]]"
library:
  - "[[scikit-learn]]"
status: reviewed
tags:
  - ridge
  - implementation
---

# scikit-learn - Ridge

## Implements

A native estimator for [[Ridge Regression]], whose defining objective is squared residual loss plus an L2 coefficient penalty.

## Support Level

**Native estimator.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
from sklearn.linear_model import Ridge
model = Ridge(alpha=1.0).fit(X, y)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `sklearn.linear_model.Ridge` |
| Fitting style | Solver-dispatch estimator; dense, sparse, and iterative routes |
| Core solver route | auto selects among direct and iterative solvers |
| Statistical inference | Limited |
| Sparse support | Yes, solver-dependent |
| GPU support | No standard route |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L2 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Solver-dispatch estimator; dense, sparse, and iterative routes
    ↓
auto selects among direct and iterative solvers
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
\text{Direct dense: O(np^2 + p^3); iterative: O(T nnz(X))}
$$

Representative additional or active space:

$$
\text{O(np + p^2) dense, route-dependent}
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

The meaning of `alpha` follows scikit-learn's unnormalized residual-sum convention; solver choice changes complexity and numerical behaviour.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Ridge Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official scikit-learn documentation for the named API or building blocks.
- [[Ridge Regression]]
- [[Ridge Regression Implementation Comparison]]

