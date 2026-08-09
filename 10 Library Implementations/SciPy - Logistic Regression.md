---
type: implementation
name: SciPy - Logistic Regression
algorithm:
  - "[[Logistic Regression]]"
library:
  - "[[SciPy]]"
status: reviewed
tags:
  - logistic
  - implementation
---

# SciPy - Logistic Regression

## Implements

A custom objective using native numerical tools for [[Logistic Regression]], whose defining objective is Bernoulli or multinomial negative log-likelihood, optionally regularized.

## Support Level

**Custom objective using native numerical tools.**

This note describes the software route separately from the mathematical model, numerical solver, backend, and hardware.

## Public API

```python
res = scipy.optimize.minimize(nll, beta0, jac=grad, method="L-BFGS-B")
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `scipy.optimize.minimize and scipy.special.expit/log_expit` |
| Fitting style | Generic likelihood optimization |
| Core solver route | BFGS, L-BFGS-B, Newton-CG, trust methods, or user choice |
| Statistical inference | Manual |
| Sparse support | Possible with custom sparse operations |
| GPU support | Normally CPU |

## Objective Mapping

The intended mathematical target is Bernoulli or multinomial negative log-likelihood, optionally regularized. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Generic likelihood optimization
    ↓
BFGS, L-BFGS-B, Newton-CG, trust methods, or user choice
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
\text{O(Tnp) first-order; curvature routes higher}
$$

Representative additional or active space:

$$
\text{Solver-dependent}
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

SciPy does not supply a logistic-regression estimator object; probability stability, regularization, intercept, and inference remain the user's responsibility.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Logistic Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official SciPy documentation for the named API or building blocks.
- [[Logistic Regression]]
- [[Logistic Regression Implementation Comparison]]

