---
type: implementation
name: PyTorch - Ridge Regression
algorithm:
  - "[[Ridge Regression]]"
library:
  - "[[PyTorch]]"
status: reviewed
tags:
  - ridge
  - implementation
---

# PyTorch - Ridge Regression

## Implements

A custom differentiable implementation for [[Ridge Regression]], whose defining objective is squared residual loss plus an L2 coefficient penalty.

## Public API

```python
model = torch.nn.Linear(p, 1)
loss = mse(pred, y) + alpha * model.weight.square().sum()
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `torch.nn.Linear plus optimizer or torch.linalg` |
| Fitting style | Iterative autograd training or direct tensor solve |
| Core solver route | Chosen optimizer or torch.linalg backend |
| Statistical inference | None automatic |
| Sparse support | Operation-specific |
| GPU support | Yes |

## Objective Mapping

The intended mathematical target is squared residual loss plus an L2 coefficient penalty. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Iterative autograd training or direct tensor solve
    ↓
Chosen optimizer or torch.linalg backend
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
\text{Iterative O(Tnp); direct O(np^2 + p^3)}
$$

Representative additional or active space:

$$
\text{Mini-batch O(bp + p), plus autograd state}
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

Optimizer `weight_decay` may have optimizer-specific semantics; exclude the bias explicitly when matching textbook ridge.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Ridge Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official PyTorch documentation for the named API or building blocks.
- [[Ridge Regression]]
- [[Ridge Regression Implementation Comparison]]
