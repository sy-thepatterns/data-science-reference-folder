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

## Notation

| Symbol | Meaning |
|---|---|
| $n$ | Number of observed examples or rows. |
| $p$ | Number of input features or columns. |
| $x_i$ | Feature vector for example $i$. |
| $y_i$ | Observed target or label for example $i$. |
| $X$ | Design matrix whose row $i$ is $x_i^T$; usually $X\in\mathbb{R}^{n\times p}$. |
| $y$ | Vector of all observed targets. |
| $\theta$ | Generic collection of parameters learned by a model. |
| $\ell$ | Loss assigned to a prediction and its observed target. |
| $\beta_0$ | Intercept: the prediction when all represented features are zero. |
| $\beta$ | Vector of $p$ coefficients; $\beta_j$ controls feature $j$ while other represented features are held fixed. |
| $\hat{\beta}$ | Estimated coefficient vector; a hat marks a quantity learned from data. |
| $X\beta$ | Vector of linear predictions before adding a separate intercept. |
| $\varepsilon$ | Unobserved error: the part of $y$ not represented by the linear mean model. |
| $r=y-X\beta$ | Residual vector: observed values minus fitted values. |
| $r_i$ | Residual for example $i$: observed minus predicted value. |
| $\sigma$ | Positive residual scale used to make errors comparable. |
| $u_i=r_i/\sigma$ | Standardized residual. |
| $\delta$ | Positive cutoff between quadratic treatment of small errors and linear treatment of large errors. |
| $\rho_\delta$ | Huber loss. |
| $\psi_\delta$ | Derivative or score of the Huber loss. |
| $w_i$ | Robust weight assigned to example $i$ in an iteratively reweighted solver. |
| $R(\beta,\sigma)$ | Estimator-specific regularization or scale term. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Small mistakes are treated like squared error because they are useful for fine adjustment. Once a mistake is very large, Huber loss stops letting it dominate the lesson. It still counts the mistake, but its influence grows like a straight line instead of exploding like a square.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Check that the quadratic and linear pieces of $\rho_\delta$ meet with the same value and slope at $|u|=\delta$.
- Differentiate each piece to obtain the bounded score function $\psi_\delta$.
- Derive the iteratively reweighted form from $w_i=\psi(u_i)/u_i$ for nonzero residuals.

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

Huber regression trades Gaussian efficiency for contamination robustness. Its statistical behavior is determined by the influence function, asymptotic variance, residual scale, and contamination fraction—not simply by whether an optimizer converges.

### Bounded residual influence

The Huber score satisfies

$$
\psi_\delta(u)=\begin{cases}u,&|u|\le\delta\\ \delta\operatorname{sign}(u),&|u|>\delta\end{cases}
$$

so a residual’s gradient contribution stops growing in magnitude after the threshold. This limits the effect of large vertical errors compared with squared loss.

### Quadratic efficiency near the center

For $|u|\le\delta$, the loss is $u^2/2$. Small residuals receive smooth quadratic treatment, allowing efficient estimation when most errors are approximately Gaussian.

### Linear tails

For $|u|>\delta$, loss grows approximately as $\delta|u|$. Large errors still matter, but their objective contribution grows linearly rather than quadratically.

### Convex optimization

The Huber loss is convex and continuously differentiable. With a linear predictor and convex coefficient penalty, every local minimum is global.

### Interpretable robust weights

The iteratively reweighted view gives weight $w_i=\psi(u_i)/u_i$. Large standardized residuals receive weights near $\delta/|u_i|$, making the source of robustness inspectable.

### Smooth bridge between loss regimes

The threshold $\delta$ controls a continuous compromise between squared-error efficiency and absolute-error robustness rather than requiring a hard decision to discard observations.

### Bounded influence under response contamination

For a location-like problem, the influence function is proportional to $\psi_\delta(u)$ and is bounded by $\delta$. An arbitrarily large residual therefore has bounded first-order effect on the estimate, unlike squared loss whose score grows without bound.

## Limitations

The limitations below identify cases where bounded residual influence is insufficient or where the robustness–efficiency trade-off is poorly chosen. Robust loss does not make the full estimator robust to every departure from the model.

### Not robust to leverage by itself

Huber bounds influence with respect to residual size, but a point with extreme $x_i$ can rotate the fitted hyperplane and acquire a small residual. Robustness in response space does not guarantee robustness in feature space.

### Threshold–scale coupling

The rule applies to standardized residual $u_i=r_i/\sigma$. If the scale estimate is distorted, the effective cutoff $\delta\sigma$ is wrong and points are downweighted too early or too late.

### Low breakdown under heavy contamination

Huber’s estimator can be overwhelmed when a substantial fraction of observations is adversarial or strategically placed. Higher-breakdown methods may be required.

### Reduced efficiency under clean Gaussian noise

Clipping scores discards some information from legitimate tail observations. A small $\delta$ increases robustness but can raise variance when the Gaussian model is correct.

### Iterative fitting

Unlike a single full-rank least-squares characterization, joint coefficient and scale estimation normally requires iteration. Statistical convergence and optimization convergence are separate concerns.

### Conditional-mean structure remains linear

Robust loss protects against some residual contamination; it does not add missing nonlinearities, interactions, or causal identification.

## Failure Modes

### Bad initial scale

An inflated scale treats outliers as ordinary residuals; a collapsed scale marks most points as outliers. Robust initialization and scale monitoring are essential.

### High-leverage masking

A cluster of extreme feature points can pull the fit toward itself and appear to have modest residuals, hiding its influence from residual-based weights.

### Threshold convention mismatch

Implementations differ in loss scaling, scale estimation, regularization, and threshold naming. Copying $\delta$ or epsilon values across them may not preserve the same estimator.

### Convergence to an inaccurate numerical solution

Tight coupling between weights, scale, and coefficients can produce slow progress. A returned result must be checked against gradient, parameter-change, or objective tolerances.

### Treating downweighting as data cleaning

A small weight does not prove an observation is erroneous. It may represent a rare but valid subgroup whose exclusion harms deployment performance.

### Contamination plus shift

A threshold tuned on one residual distribution may behave poorly after variance or tail behavior changes, causing widespread downweighting or renewed sensitivity.

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

