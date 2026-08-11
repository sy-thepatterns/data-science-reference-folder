---
type: implementation
name: scikit-learn - LogisticRegression
algorithm:
  - "[[Logistic Regression]]"
library:
  - "[[scikit-learn]]"
status: reviewed
tags:
  - logistic
  - implementation
---

# scikit-learn - LogisticRegression

## Implements

A native classification estimator for [[Logistic Regression]], whose defining objective is Bernoulli or multinomial negative log-likelihood, optionally regularized.

## Public API

```python
from sklearn.linear_model import LogisticRegression
model = LogisticRegression().fit(X, y)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `sklearn.linear_model.LogisticRegression` |
| Fitting style | Regularized binary or multinomial optimization |
| Core solver route | LBFGS, liblinear, Newton, SAG, or SAGA |
| Statistical inference | Limited |
| Sparse support | Yes, solver/input dependent |
| GPU support | No standard route |

## Objective Mapping

The intended mathematical target is Bernoulli or multinomial negative log-likelihood, optionally regularized. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Regularized binary or multinomial optimization
    ↓
LBFGS, liblinear, Newton, SAG, or SAGA
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
\text{First-order O(Tnp); Newton routes may use O(np^2 + p^3)}
$$

Representative additional or active space:

$$
\text{Solver-dependent; Hessian routes can use O(p^2)}
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

Regularization is applied by default and solver/penalty compatibility matters; this differs from unregularized textbook MLE.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Logistic Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official scikit-learn documentation for the named API or building blocks.
- [[Logistic Regression]]
- [[Logistic Regression Implementation Comparison]]
