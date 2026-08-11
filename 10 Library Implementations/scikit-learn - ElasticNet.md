---
type: implementation
name: scikit-learn - ElasticNet
algorithm:
  - "[[Elastic Net]]"
library:
  - "[[scikit-learn]]"
status: reviewed
tags:
  - elastic-net
  - implementation
---

# scikit-learn - ElasticNet

## Implements

A native estimator for [[Elastic Net]], whose defining objective is squared residual loss plus mixed L1 and L2 coefficient penalties.

## Public API

```python
from sklearn.linear_model import ElasticNet
model = ElasticNet(alpha=0.1, l1_ratio=0.5).fit(X, y)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `sklearn.linear_model.ElasticNet` |
| Fitting style | Coordinate descent with dual-gap stopping |
| Core solver route | Coordinate descent |
| Statistical inference | Limited |
| Sparse support | Sparse coefficients |
| GPU support | No standard route |

## Objective Mapping

The intended mathematical target is squared residual loss plus mixed L1 and L2 coefficient penalties. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Coordinate descent with dual-gap stopping
    ↓
Coordinate descent
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
\text{O(Tnp) dense high-level}
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

`alpha` and `l1_ratio` jointly define penalty weights; near-pure L2 settings may be numerically inefficient in this estimator.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Elastic Net Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official scikit-learn documentation for the named API or building blocks.
- [[Elastic Net]]
- [[Elastic Net Implementation Comparison]]
