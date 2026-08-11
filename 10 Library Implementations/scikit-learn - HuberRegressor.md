---
type: implementation
name: scikit-learn - HuberRegressor
algorithm:
  - "[[Huber Regression]]"
library:
  - "[[scikit-learn]]"
status: reviewed
tags:
  - huber
  - implementation
---

# scikit-learn - HuberRegressor

## Implements

A native scale-aware estimator for [[Huber Regression]], whose defining objective is a linear predictor fitted with Huber residual loss.

## Public API

```python
from sklearn.linear_model import HuberRegressor
model = HuberRegressor(epsilon=1.35).fit(X, y)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `sklearn.linear_model.HuberRegressor` |
| Fitting style | Joint coefficient, intercept, and scale optimization with L2 penalty |
| Core solver route | L-BFGS-B route |
| Statistical inference | Limited |
| Sparse support | Dense input route |
| GPU support | No |

## Objective Mapping

The intended mathematical target is a linear predictor fitted with Huber residual loss. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Joint coefficient, intercept, and scale optimization with L2 penalty
    ↓
L-BFGS-B route
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
\text{O(Tnp) high-level}
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

This objective estimates scale and includes an L2 penalty; it is not identical to merely replacing MSE with a fixed-delta Huber loss.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Huber Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official scikit-learn documentation for the named API or building blocks.
- [[Huber Regression]]
- [[Huber Regression Implementation Comparison]]
