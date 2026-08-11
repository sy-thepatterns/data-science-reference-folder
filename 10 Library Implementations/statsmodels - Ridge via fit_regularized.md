---
type: implementation
name: statsmodels - Ridge via fit_regularized
algorithm:
  - "[[Ridge Regression]]"
library:
  - "[[statsmodels]]"
status: reviewed
tags:
  - ridge
  - implementation
---

# statsmodels - Ridge via fit_regularized

## Implements

A native regularized fitting route for [[Ridge Regression]], whose defining objective is squared residual loss plus an L2 coefficient penalty.

## Public API

```python
import statsmodels.api as sm
X1 = sm.add_constant(X)
res = sm.OLS(y, X1).fit_regularized(alpha=alpha, L1_wt=0.0)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `statsmodels.OLS.fit_regularized` |
| Fitting style | Elastic-net interface with pure L2 setting |
| Core solver route | statsmodels regularized optimization |
| Statistical inference | Regularized result inference is limited compared with OLS |
| Sparse support | Limited |
| GPU support | No |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L2 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Elastic-net interface with pure L2 setting
    ↓
statsmodels regularized optimization
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
\text{Typically iterative; O(Tnp) high-level}
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

Exclude or separately weight the intercept if it should remain unpenalized; this route is not the same results object as ordinary OLS.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Ridge Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official statsmodels documentation for the named API or building blocks.
- [[Ridge Regression]]
- [[Ridge Regression Implementation Comparison]]
