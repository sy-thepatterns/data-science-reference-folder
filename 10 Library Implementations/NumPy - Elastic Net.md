---
type: implementation
name: NumPy - Elastic Net
algorithm:
  - "[[Elastic Net]]"
library:
  - "[[NumPy]]"
status: reviewed
tags:
  - elastic-net
  - implementation
---

# NumPy - Elastic Net

## Implements

A custom construction for [[Elastic Net]], whose defining objective is squared residual loss plus mixed L1 and L2 coefficient penalties.

## Support Level

**Custom construction.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
beta = soft_threshold(beta - step * grad, step * l1) / (1 + step * l2)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `Array operations in custom proximal or coordinate solver` |
| Fitting style | Soft thresholding plus L2 shrinkage |
| Core solver route | User-authored optimizer |
| Statistical inference | None automatic |
| Sparse support | Manual |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is squared residual loss plus mixed L1 and L2 coefficient penalties. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Soft thresholding plus L2 shrinkage
    ↓
User-authored optimizer
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
\text{O(Tnp)}
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

There is no native NumPy estimator; objective scaling and convergence checks must be implemented and tested.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Elastic Net Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official NumPy documentation for the named API or building blocks.
- [[Elastic Net]]
- [[Elastic Net Implementation Comparison]]

