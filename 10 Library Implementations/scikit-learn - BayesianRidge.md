---
type: implementation
name: scikit-learn - BayesianRidge
algorithm:
  - "[[Bayesian Linear Regression]]"
library:
  - "[[scikit-learn]]"
status: reviewed
tags:
  - bayesian
  - implementation
---

# scikit-learn - BayesianRidge

## Implements

A native empirical-bayes estimator for [[Bayesian Linear Regression]], whose defining objective is posterior and posterior-predictive inference for a linear Gaussian model.

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
| $p(\theta)$ | Prior distribution: uncertainty about parameters before conditioning on the current data. |
| $p(\mathcal{D}\mid\theta)$ | Likelihood: probability model for the observed data when parameters are fixed. |
| $p(\theta\mid\mathcal{D})$ | Posterior distribution after combining prior and likelihood. |
| $\sigma^2$ | Observation-noise variance. |
| $m_0,S_0$ | Prior mean and covariance of the coefficient vector. |
| $m_n,S_n$ | Posterior mean and covariance after $n$ observations. |
| $x_\star,y_\star$ | A new feature vector and its not-yet-observed target. |
| $\mathcal{N}(m,S)$ | Gaussian distribution with mean $m$ and covariance $S$. |
| $\tau^2$ | Prior coefficient variance in an isotropic Gaussian prior. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Start with a range of plausible coefficient values instead of pretending you know one exact answer. Data shifts weight toward values that explain what you saw. Prediction then averages over the remaining possibilities, so uncertainty grows naturally when the data leave several explanations alive.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Derive Bayes' rule from the two factorizations of a joint distribution: $p(\theta,\mathcal{D})=p(\mathcal{D}\mid\theta)p(\theta)=p(\theta\mid\mathcal{D})p(\mathcal{D})$.
- Derive Gaussian conjugacy by expanding the log prior plus log likelihood and completing the square in $\beta$.
- Derive the posterior predictive mean and variance with the laws of total expectation and total variance.

## Public API

```python
from sklearn.linear_model import BayesianRidge
model = BayesianRidge().fit(X, y)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `sklearn.linear_model.BayesianRidge` |
| Fitting style | Iterative evidence maximization for isotropic coefficient and noise precisions |
| Core solver route | MacKay/Tipping-style fixed-point updates with dense linear algebra |
| Statistical inference | Predictive standard deviation and coefficient covariance |
| Sparse support | Dense route |
| GPU support | No |

## Objective Mapping

The intended mathematical target is posterior and posterior-predictive inference for a linear Gaussian model. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Iterative evidence maximization for isotropic coefficient and noise precisions
    ↓
MacKay/Tipping-style fixed-point updates with dense linear algebra
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
\text{Approximately O(T(np^2 + p^3)) in coefficient space}
$$

Representative additional or active space:

$$
\text{O(np + p^2)}
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

This is a specific Bayesian ridge model with hyperparameters estimated from data, not a general prior/likelihood programming interface.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Bayesian Linear Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official scikit-learn documentation for the named API or building blocks.
- [[Bayesian Linear Regression]]
- [[Bayesian Linear Regression Implementation Comparison]]
