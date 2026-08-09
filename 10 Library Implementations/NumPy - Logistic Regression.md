---
type: implementation
name: NumPy - Logistic Regression
algorithm:
  - "[[Logistic Regression]]"
library:
  - "[[NumPy]]"
status: reviewed
tags:
  - logistic
  - implementation
---

# NumPy - Logistic Regression

## Implements

A custom construction for [[Logistic Regression]], whose defining objective is Bernoulli or multinomial negative log-likelihood, optionally regularized.

## Support Level

**Custom construction.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
p_hat = 1 / (1 + np.exp(-X @ beta))
grad = X.T @ (p_hat - y) / n
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `Array operations in custom likelihood optimizer` |
| Fitting style | User-authored gradient, Newton, or IRLS loop |
| Core solver route | numpy.linalg solve for Newton systems |
| Statistical inference | Manual |
| Sparse support | No native sparse arrays |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is Bernoulli or multinomial negative log-likelihood, optionally regularized. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
User-authored gradient, Newton, or IRLS loop
    ↓
numpy.linalg solve for Newton systems
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
\text{Gradient O(Tnp); Newton O(T(np^2 + p^3))}
$$

Representative additional or active space:

$$
\text{O(np + p^2) for Newton}
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

Naive sigmoid and log calculations can overflow; use stable log-add-exp identities and handle separation.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Logistic Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official NumPy documentation for the named API or building blocks.
- [[Logistic Regression]]
- [[Logistic Regression Implementation Comparison]]

