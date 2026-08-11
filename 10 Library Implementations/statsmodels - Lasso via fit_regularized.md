---
type: implementation
name: statsmodels - Lasso via fit_regularized
algorithm:
  - "[[Lasso Regression]]"
library:
  - "[[statsmodels]]"
status: reviewed
tags:
  - lasso
  - implementation
---

# statsmodels - Lasso via fit_regularized

## Implements

A native regularized fitting route for [[Lasso Regression]], whose defining objective is squared residual loss plus an L1 coefficient penalty.

## Public API

```python
res = sm.OLS(y, X1).fit_regularized(alpha=alpha, L1_wt=1.0)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `statsmodels.OLS.fit_regularized` |
| Fitting style | Elastic-net interface with pure L1 setting |
| Core solver route | Coordinate-descent-like regularized route |
| Statistical inference | Limited after selection |
| Sparse support | Limited |
| GPU support | No |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L1 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Elastic-net interface with pure L1 setting
    ↓
Coordinate-descent-like regularized route
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

Intercept handling, refitting, trimming, and alpha scaling require explicit choices; ordinary post-selection standard errors are not automatically valid.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Lasso Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official statsmodels documentation for the named API or building blocks.
- [[Lasso Regression]]
- [[Lasso Regression Implementation Comparison]]
