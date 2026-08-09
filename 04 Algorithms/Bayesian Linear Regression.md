---
type: algorithm
name: Bayesian Linear Regression
learning_paradigm:
  - "[[Supervised Learning]]"
task:
  - "[[Regression]]"
family:
  - "[[Linear Models]]"
  - "[[Bayesian Methods]]"
foundations:
  - "[[Linear Regression]]"
  - Bayes Theorem
  - "[[Expected Value]]"
  - "[[Variance]]"
objective:
  - Posterior inference
loss: []
optimization: []
solvers:
  - Cholesky Decomposition
  - "[[QR Decomposition]]"
  - "[[Singular Value Decomposition]]"
implementations:
  - "[[scikit-learn - BayesianRidge]]"
  - "[[statsmodels - Bayesian Linear Regression]]"
  - "[[NumPy - Bayesian Linear Regression]]"
  - "[[SciPy - Bayesian Linear Regression]]"
  - "[[PyTorch - Bayesian Linear Regression]]"
  - "[[TensorFlow - Bayesian Linear Regression]]"
related:
  - "[[Linear Regression]]"
  - "[[Ridge Regression]]"
status: reviewed
tags:
  - linear
  - parametric
  - probabilistic
  - bayesian
  - interpretable
---

# Bayesian Linear Regression

## Overview

Bayesian linear regression places probability distributions over coefficients and, commonly, noise parameters. Bayes' theorem combines a likelihood with prior information to form a posterior distribution. Predictions average over posterior parameter uncertainty rather than relying only on one fitted coefficient vector.

The model, exact conjugate inference, approximate inference algorithm, numerical linear-algebra routine, library implementation, and hardware backend are separate layers.

## Problem Definition

Given:

$$
X\in\mathbb{R}^{n\times p}
$$

and:

$$
y\in\mathbb{R}^{n}
$$

infer:

$$
p(\beta,\sigma^2\mid X,y)
$$

or an approximation to it, and use the posterior predictive distribution for new outcomes.

## Gaussian Conjugate Model

Assume the likelihood:

$$
y\mid X,\beta,\sigma^2
\sim
\mathcal{N}(X\beta,\sigma^2I)
$$

For known noise variance, use a Gaussian prior:

$$
\beta
\sim
\mathcal{N}(m_0,S_0)
$$

Bayes' theorem gives:

$$
p(\beta\mid X,y,\sigma^2)
\propto
p(y\mid X,\beta,\sigma^2)p(\beta)
$$

The posterior is Gaussian:

$$
\beta\mid X,y,\sigma^2
\sim
\mathcal{N}(m_n,S_n)
$$

with posterior covariance:

$$
S_n
=
\left(
S_0^{-1}
+
\frac{1}{\sigma^2}X^TX
\right)^{-1}
$$

and posterior mean:

$$
m_n
=
S_n
\left(
S_0^{-1}m_0
+
\frac{1}{\sigma^2}X^Ty
\right)
$$

These inverse expressions characterize the distribution. Stable software solves linear systems or uses matrix factorizations.

## Posterior Predictive Distribution

For a new feature vector:

$$
x_{\star}\in\mathbb{R}^{p}
$$

the predictive distribution integrates over coefficients:

$$
p(y_{\star}\mid x_{\star},X,y)
=
\int
p(y_{\star}\mid x_{\star},\beta,\sigma^2)
p(\beta\mid X,y,\sigma^2)
\,d\beta
$$

Under the known-variance conjugate model:

$$
y_{\star}\mid x_{\star},X,y
\sim
\mathcal{N}
\left(
x_{\star}^Tm_n,
\sigma^2+x_{\star}^TS_nx_{\star}
\right)
$$

The first variance term represents observation noise. The second represents posterior coefficient uncertainty.

## Unknown Noise Variance

A conjugate normal-inverse-gamma prior yields a normal-inverse-gamma posterior and a Student predictive distribution after integrating out noise variance. Alternative priors may require approximate inference.

## Relationship to Ridge Regression

With zero prior mean and isotropic prior covariance:

$$
S_0
=
\tau^2I
$$

the posterior mode and mean for known variance solve a ridge-like system. The regularization ratio is determined by prior and noise scales. [[Ridge Regression]] reports a point estimate; Bayesian linear regression retains a posterior distribution and posterior predictive uncertainty.

## Statistical Properties

### Bias and Shrinkage

Informative or finite-variance priors shrink estimates toward prior structure. Frequentist bias can be introduced while posterior or predictive risk improves.

### Uncertainty

Credible intervals are posterior probability statements conditional on the model and prior. They are not automatically frequentist confidence intervals.

### Identifiability

Proper priors can produce a proper posterior in settings where ordinary least-squares coefficients are not uniquely identified, but posterior conclusions may then be strongly prior-sensitive.

### Calibration

Posterior uncertainty is only as credible as the likelihood, prior, and computation. Misspecified noise, unmodelled dependence, or approximate-inference error can lead to poor calibration.

## Inference Methods

Conjugate Gaussian models admit exact algebraic updates. Nonconjugate priors or likelihoods may use Markov chain Monte Carlo, variational inference, Laplace approximation, or expectation propagation. These are inference algorithms, not synonymous with Bayesian linear regression.

## Training Pseudocode

```text
INPUT:
    design matrix X
    target y
    likelihood specification
    coefficient and noise priors
    exact or approximate inference method

1. Validate the probability model and data shapes.
2. Construct the likelihood and priors.
3. If conjugate, compute posterior natural parameters using stable solves.
4. Otherwise, run the selected inference algorithm.
5. Check numerical, sampling, or approximation diagnostics.
6. Store the posterior representation and preprocessing metadata.

OUTPUT:
    posterior distribution or approximation
```

## Complexity

For dense conjugate inference in coefficient space, constructing the precision matrix costs approximately:

$$
O(np^2)
$$

and factorizing it costs:

$$
O(p^3)
$$

with dense posterior covariance storage:

$$
O(p^2)
$$

Alternative sample-space formulations can be preferable when the feature count exceeds the sample count. Approximate inference depends on iterations, samples, chains, sparsity, and convergence. A predictive mean for:

$$
m
$$

rows costs:

$$
O(mp)
$$

while full predictive variances with a dense covariance can cost:

$$
O(mp^2)
$$

## Hyperparameters

| Parameter | Mathematical effect | Typical behaviour |
|---|---|---|
| Prior mean | Sets shrinkage centre | Strong priors pull coefficients toward it |
| Prior covariance | Sets direction and strength of prior uncertainty | Smaller variance increases shrinkage |
| Noise prior | Controls uncertainty over residual variance | Influences predictive tails and intervals |
| Approximation controls | Determine computational accuracy | More samples or tighter tolerances increase work |

## Advantages

- Represents parameter and predictive uncertainty.
- Incorporates prior information explicitly.
- Regularizes ill-conditioned or high-dimensional problems.
- Supports coherent sequential updating under a stable model.

## Limitations and Failure Modes

- Conclusions can be sensitive to prior choice.
- Dense exact inference scales poorly with feature count.
- Approximate inference introduces convergence or approximation error.
- Gaussian noise may be inappropriate for outliers or heteroscedasticity.
- Credible intervals can be misleading under model misspecification.
- Comparing priors after seeing test results can leak evaluation information.

## Diagnostics

- Perform prior predictive checks before fitting.
- Perform posterior predictive checks after fitting.
- Conduct sensitivity analysis over plausible priors.
- For sampling, inspect convergence and effective sample sizes.
- For approximations, compare against exact or sampling results on smaller cases.
- Evaluate predictive calibration and interval coverage empirically.

## Related Algorithms

- [[Linear Regression]] commonly returns an ordinary least-squares point estimate.
- [[Ridge Regression]] matches a Gaussian-prior posterior mode under specific assumptions.
- Robust Bayesian regression replaces the Gaussian noise model with a heavy-tailed alternative.

## Implementations

- [[scikit-learn - BayesianRidge]]
- [[statsmodels - Bayesian Linear Regression]]
- [[NumPy - Bayesian Linear Regression]]
- [[SciPy - Bayesian Linear Regression]]
- [[PyTorch - Bayesian Linear Regression]]
- [[TensorFlow - Bayesian Linear Regression]]
- [[Bayesian Linear Regression Implementation Comparison]]

## References

- Lindley, D. V., and Smith, A. F. M. (1972). *Bayes Estimates for the Linear Model*.
- Gelman, A. et al. *Bayesian Data Analysis*.
- Bishop, C. M. *Pattern Recognition and Machine Learning*.

