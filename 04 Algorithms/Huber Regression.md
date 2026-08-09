---
type: algorithm
name: Huber Regression
aliases:
  - Huber M-Estimation Regression
learning_paradigm:
  - "[[Supervised Learning]]"
task:
  - "[[Regression]]"
family:
  - "[[Linear Models]]"
foundations:
  - "[[Linear Regression]]"
objective:
  - Huber M-estimation
loss:
  - Huber Loss
optimization:
  - Convex optimization
solvers:
  - Iteratively Reweighted Least Squares
  - Gradient-Based Optimization
implementations:
  - "[[scikit-learn - HuberRegressor]]"
  - "[[statsmodels - RLM with HuberT]]"
  - "[[NumPy - Huber Regression]]"
  - "[[SciPy - Huber Regression]]"
  - "[[PyTorch - Huber Regression]]"
  - "[[TensorFlow - Huber Regression]]"
related:
  - "[[Linear Regression]]"
  - "[[Ridge Regression]]"
status: reviewed
tags:
  - linear
  - parametric
  - frequentist
  - convex
  - differentiable
  - interpretable
  - robust
---

# Huber Regression

## Overview

Huber regression fits a linear predictor using a loss that is quadratic for small standardized residuals and linear for large ones. Compared with [[Linear Regression]] under squared loss, it reduces the influence of large response residuals while retaining smooth behaviour near the optimum.

It is robust to vertical outliers, not automatically to high-leverage points in the feature space.

## Problem Definition

For residual:

$$
r_i
=
y_i-\beta_0-x_i^T\beta
$$

and positive scale:

$$
\sigma>0
$$

define the standardized residual:

$$
u_i
=
\frac{r_i}{\sigma}
$$

## Huber Loss

For threshold:

$$
\delta>0
$$

the Huber loss is:

$$
\rho_{\delta}(u)
=
\begin{cases}
\frac{1}{2}u^2, & |u|\le\delta\\
\delta|u|-\frac{1}{2}\delta^2, & |u|>\delta
\end{cases}
$$

Its score function is:

$$
\psi_{\delta}(u)
=
\rho_{\delta}'(u)
=
\begin{cases}
u, & |u|\le\delta\\
\delta\operatorname{sign}(u), & |u|>\delta
\end{cases}
$$

The bounded score prevents a single large residual from contributing an arbitrarily large gradient.

## Formal Definition

A scale-aware objective is:

$$
(\hat{\beta}_0,\hat{\beta},\hat{\sigma})
=
\arg\min_{\beta_0,\beta,\sigma>0}
\left\{
\sum_{i=1}^{n}
\sigma^2
\rho_{\delta}
\left(
\frac{y_i-\beta_0-x_i^T\beta}{\sigma}
\right)
+
R(\beta,\sigma)
\right\}
$$

where the exact scale term or regularizer:

$$
R(\beta,\sigma)
$$

depends on the estimator definition. Some implementations fix scale, estimate it jointly, or add a coefficient penalty. Those are distinct objective conventions.

## Iteratively Reweighted View

For nonzero standardized residuals, define:

$$
w_i
=
\frac{\psi_{\delta}(u_i)}{u_i}
=
\begin{cases}
1, & |u_i|\le\delta\\
\frac{\delta}{|u_i|}, & |u_i|>\delta
\end{cases}
$$

Large residuals receive smaller weights. Iteratively reweighted least squares alternates weight calculation with weighted least-squares updates. It is one solver strategy, not the definition of Huber regression.

## Statistical Properties

### Robustness

The loss has bounded influence in the response-residual direction. Its classical breakdown point can nevertheless be low, and leverage contamination can still dominate the fit.

### Efficiency

When errors are nearly Gaussian and the threshold is chosen conventionally, Huber regression can retain high efficiency relative to least squares. Under heavy-tailed contamination, it may have much lower variance than squared-loss regression.

### Bias

Robustness does not remove bias from omitted variables, measurement error, sample selection, or misspecified conditional structure.

## Training Pseudocode

```text
INPUT:
    design matrix X
    target y
    Huber threshold delta
    scale-estimation rule
    selected convex solver

1. Validate data and initialize coefficients and scale.
2. Compute standardized residuals.
3. Evaluate the Huber objective or robust weights.
4. Update coefficients using the selected solver.
5. Update scale if the estimator defines a scale update.
6. Repeat until the stopping criterion is satisfied.
7. Store coefficients, intercept, scale, and convergence diagnostics.

OUTPUT:
    robust linear-model estimate
```

## Complexity

There is no single solver-independent complexity. For iteratively reweighted least squares, each of:

$$
T
$$

outer iterations computes residuals in:

$$
O(np)
$$

and solves a weighted least-squares problem. A dense normal-equation-style route is approximately:

$$
O\left(T(np^2+p^3)\right)
$$

while iterative sparse routes depend on nonzero count and inner convergence. Dense batch prediction costs:

$$
O(mp)
$$

## Hyperparameters

| Parameter | Mathematical effect | Typical behaviour |
|---|---|---|
| Huber threshold | Sets transition from quadratic to linear loss | Smaller values increase robustness and reduce Gaussian efficiency |
| Scale rule | Defines residual standardization | Poor scale estimates distort the effective threshold |
| Coefficient penalty | Adds shrinkage if present | Changes the estimator beyond pure Huber fitting |
| Solver tolerance | Controls stopping | Tighter values require more work |

## Advantages

- Convex residual loss.
- Less sensitive to large response residuals than squared loss.
- Retains quadratic sensitivity near zero.
- Linear prediction remains interpretable and inexpensive.

## Limitations and Failure Modes

- Does not inherently protect against high-leverage feature outliers.
- Threshold and scale conventions vary across implementations.
- Heavy contamination may require higher-breakdown estimators.
- Optimization is iterative rather than a single ordinary least-squares solve.
- Feature scaling and regularization selection can leak validation data.

## Diagnostics

- Compare residual distributions and robust weights.
- Inspect leverage as well as residual magnitude.
- Verify convergence and scale stability.
- Compare with ordinary least squares on clean and contaminated subsets.
- Perform sensitivity analysis over the threshold.

## Related Algorithms

- [[Linear Regression]] uses fully quadratic residual loss.
- [[Ridge Regression]] addresses coefficient instability, not residual robustness.
- Least absolute deviations uses an absolute residual loss everywhere.

## Implementations

- [[scikit-learn - HuberRegressor]]
- [[statsmodels - RLM with HuberT]]
- [[NumPy - Huber Regression]]
- [[SciPy - Huber Regression]]
- [[PyTorch - Huber Regression]]
- [[TensorFlow - Huber Regression]]
- [[Huber Regression Implementation Comparison]]

## References

- Huber, P. J. (1964). *Robust Estimation of a Location Parameter*.
- Huber, P. J., and Ronchetti, E. M. *Robust Statistics*.

