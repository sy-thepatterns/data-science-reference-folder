---
type: implementation
name: SciPy - Huber Regression
algorithm:
  - "[[Huber Regression]]"
library:
  - "[[SciPy]]"
status: reviewed
tags:
  - huber
  - implementation
---

# SciPy - Huber Regression

## Implements

A custom objective from native building blocks for [[Huber Regression]], whose defining objective is a linear predictor fitted with Huber residual loss.

## Public API

```python
loss = scipy.special.huber(delta, y - X @ beta).sum()
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `scipy.special.huber plus scipy.optimize.minimize` |
| Fitting style | Generic convex optimization |
| Core solver route | Chosen scipy.optimize method |
| Statistical inference | None automatic |
| Sparse support | Possible with custom sparse operations |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is a linear predictor fitted with Huber residual loss. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Generic convex optimization
    ↓
Chosen scipy.optimize method
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
\text{O(Tnp) for first-order evaluations; solver-dependent}
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

`scipy.special.huber` evaluates the loss only; it does not fit coefficients or estimate scale.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Huber Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official SciPy documentation for the named API or building blocks.
- [[Huber Regression]]
- [[Huber Regression Implementation Comparison]]
