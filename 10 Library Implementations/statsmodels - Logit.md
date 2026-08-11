---
type: implementation
name: statsmodels - Logit
algorithm:
  - "[[Logistic Regression]]"
library:
  - "[[statsmodels]]"
status: reviewed
tags:
  - logistic
  - implementation
---

# statsmodels - Logit

## Implements

A native statistical model for [[Logistic Regression]], whose defining objective is Bernoulli or multinomial negative log-likelihood, optionally regularized.

## Public API

```python
X1 = sm.add_constant(X)
res = sm.Logit(y, X1).fit()
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `statsmodels.Logit or statsmodels.GLM with Binomial family` |
| Fitting style | Maximum-likelihood estimation with inferential results |
| Core solver route | Newton or selected likelihood optimizer |
| Statistical inference | Extensive |
| Sparse support | Limited |
| GPU support | No |

## Objective Mapping

The intended mathematical target is Bernoulli or multinomial negative log-likelihood, optionally regularized. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Maximum-likelihood estimation with inferential results
    ↓
Newton or selected likelihood optimizer
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
\text{O(T(np^2 + p^3)) for dense Newton-like fitting}
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

An intercept is normally added explicitly; separation and covariance assumptions require diagnostic attention.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Logistic Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official statsmodels documentation for the named API or building blocks.
- [[Logistic Regression]]
- [[Logistic Regression Implementation Comparison]]
