---
type: algorithm-family
name: Bayesian Methods
parent_family: []
tasks: []
members:
  - "[[Bayesian Linear Regression]]"
  - "[[Gaussian Process]]"
related:
  - "[[Probabilistic Models]]"
  - "[[Linear Models]]"
status: complete
tags:
  - bayesian-methods
---

# Bayesian Methods

## Definition

Use probability distributions to represent uncertainty about unknown quantities and update them by conditioning on observed data.

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

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

$p(\theta\mid\mathcal{D})\propto p(\mathcal{D}\mid\theta)p(\theta)$, with predictions obtained by posterior integration.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Bayesian Linear Regression]]
- [[Gaussian Process]]

## Family Tree

```text
Bayesian Methods
├── Bayesian Linear Regression
├── Gaussian Process
└── Gaussian Process
```

## Shared Strengths

Explicit uncertainty, prior information, hierarchical structure, and coherent sequential updating.

## Shared Limitations

Misspecification and prior sensitivity remain; exact inference is often unavailable, and approximation diagnostics are essential.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Bayesian Linear Regression]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Gaussian Process]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Probabilistic Models]]
- [[Linear Models]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
