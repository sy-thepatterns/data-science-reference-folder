---
type: implementation
name: PyTorch - Logistic Regression
algorithm:
  - "[[Logistic Regression]]"
library:
  - "[[PyTorch]]"
status: reviewed
tags:
  - logistic
  - implementation
---

# PyTorch - Logistic Regression

## Implements

A custom differentiable implementation for [[Logistic Regression]], whose defining objective is Bernoulli or multinomial negative log-likelihood, optionally regularized.

## Support Level

**Custom differentiable implementation.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
model = torch.nn.Linear(p, 1)
loss_fn = torch.nn.BCEWithLogitsLoss()
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `torch.nn.Linear and torch.nn.BCEWithLogitsLoss` |
| Fitting style | Mini-batch or full-batch iterative training |
| Core solver route | Chosen optimizer |
| Statistical inference | None automatic |
| Sparse support | Operation-specific |
| GPU support | Yes |

## Objective Mapping

The intended mathematical target is Bernoulli or multinomial negative log-likelihood, optionally regularized. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Mini-batch or full-batch iterative training
    ↓
Chosen optimizer
    ↓
PyTorch numerical operations and dependencies
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
\text{O(Tnp) for full passes}
$$

Representative additional or active space:

$$
\text{O(bp + p), plus optimizer state}
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

Use logits directly for numerical stability; thresholding, calibration, and coefficient inference are separate concerns.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Logistic Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official PyTorch documentation for the named API or building blocks.
- [[Logistic Regression]]
- [[Logistic Regression Implementation Comparison]]

