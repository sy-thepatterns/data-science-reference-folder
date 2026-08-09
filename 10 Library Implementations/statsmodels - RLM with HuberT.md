---
type: implementation
name: statsmodels - RLM with HuberT
algorithm:
  - "[[Huber Regression]]"
library:
  - "[[statsmodels]]"
status: reviewed
tags:
  - huber
  - implementation
---

# statsmodels - RLM with HuberT

## Implements

A native robust linear model for [[Huber Regression]], whose defining objective is a linear predictor fitted with Huber residual loss.

## Support Level

**Native robust linear model.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
model = sm.RLM(y, X1, M=sm.robust.norms.HuberT())
res = model.fit()
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `statsmodels.RLM and statsmodels.robust.norms.HuberT` |
| Fitting style | M-estimation with robust scale and iteratively reweighted fitting |
| Core solver route | IRLS |
| Statistical inference | Robust-model result summaries |
| Sparse support | Limited |
| GPU support | No |

## Objective Mapping

The intended mathematical target is a linear predictor fitted with Huber residual loss. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
M-estimation with robust scale and iteratively reweighted fitting
    ↓
IRLS
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
\text{O(T(np^2 + p^3)) for dense weighted solves}
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

Its scale, norm, covariance, and stopping definitions differ from scikit-learn's HuberRegressor.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Huber Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official statsmodels documentation for the named API or building blocks.
- [[Huber Regression]]
- [[Huber Regression Implementation Comparison]]

