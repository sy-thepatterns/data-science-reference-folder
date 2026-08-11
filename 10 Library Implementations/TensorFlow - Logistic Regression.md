---
type: implementation
name: TensorFlow - Logistic Regression
algorithm:
  - "[[Logistic Regression]]"
library:
  - "[[TensorFlow]]"
status: reviewed
tags:
  - logistic
  - implementation
---

# TensorFlow - Logistic Regression

## Implements

A custom differentiable implementation for [[Logistic Regression]], whose defining objective is Bernoulli or multinomial negative log-likelihood, optionally regularized.

## Public API

```python
model = tf.keras.Sequential([tf.keras.layers.Dense(1)])
loss = tf.keras.losses.BinaryCrossentropy(from_logits=True)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `tf.keras Dense plus BinaryCrossentropy` |
| Fitting style | Keras iterative training |
| Core solver route | Chosen Keras optimizer |
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
Keras iterative training
    ↓
Chosen Keras optimizer
    ↓
TensorFlow numerical operations and dependencies
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

A sigmoid activation plus `from_logits=True` is incorrect; keep logits or set the loss convention consistently.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Logistic Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official TensorFlow documentation for the named API or building blocks.
- [[Logistic Regression]]
- [[Logistic Regression Implementation Comparison]]
