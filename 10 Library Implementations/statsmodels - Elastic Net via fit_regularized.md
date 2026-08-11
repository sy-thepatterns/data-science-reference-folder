---
type: implementation
name: statsmodels - Elastic Net via fit_regularized
algorithm:
  - "[[Elastic Net]]"
library:
  - "[[statsmodels]]"
status: reviewed
tags:
  - elastic-net
  - implementation
---

# statsmodels - Elastic Net via fit_regularized

## Implements

A native regularized fitting route for [[Elastic Net]], whose defining objective is squared residual loss plus mixed L1 and L2 coefficient penalties.

## Public API

```python
res = sm.OLS(y, X1).fit_regularized(alpha=alpha, L1_wt=l1_wt)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `statsmodels.OLS.fit_regularized` |
| Fitting style | Elastic-net regularization interface |
| Core solver route | statsmodels regularized route |
| Statistical inference | Limited |
| Sparse support | Limited |
| GPU support | No |

## Objective Mapping

The intended mathematical target is squared residual loss plus mixed L1 and L2 coefficient penalties. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Elastic-net regularization interface
    ↓
statsmodels regularized route
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

`L1_wt` and alpha conventions differ from scikit-learn's parameterization; intercept penalty and post-fit inference need explicit treatment.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Elastic Net Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official statsmodels documentation for the named API or building blocks.
- [[Elastic Net]]
- [[Elastic Net Implementation Comparison]]
