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

### Full parameter uncertainty

Rather than returning only $\hat\beta$, Bayesian linear regression estimates $p(\beta,\sigma^2\mid X,y)$. Correlations and uncertainty among coefficients remain available for prediction and decision-making.

### Posterior predictive uncertainty

Prediction integrates over coefficient uncertainty:

$$
p(y_\star\mid x_\star,X,y)=\int p(y_\star\mid x_\star,\beta,\sigma^2)p(\beta,\sigma^2\mid X,y)\,d\beta\,d\sigma^2
$$

so intervals can include both observation noise and uncertainty about the fitted mean.

### Regularization through priors

A Gaussian prior contributes prior precision to $X^TX/\sigma^2$. Proper prior precision can stabilize weakly identified directions and produce a proper posterior when ordinary least-squares coefficients are nonunique.

### Coherent sequential updating

Under a stable model, today’s posterior can serve as tomorrow’s prior. Bayes’ rule combines information without distinguishing an arbitrary first and second dataset.

### Hierarchical modelling

Priors may share information across groups, tasks, or coefficients through learned hyperparameters. Partial pooling adapts shrinkage to the evidence rather than fitting every group independently.

### Decision-theoretic output

A posterior distribution can be combined with an explicit utility or loss to choose actions. Parameter estimation and decision policy remain separate layers.

### Exact reference cases

Conjugate Gaussian models admit analytic posteriors and predictive distributions. These provide interpretable baselines and test cases for approximate inference software.

## Limitations

### Model-dependent uncertainty

Posterior probability is conditional on the likelihood, prior, feature representation, and data being an adequate model. A narrow posterior can be confidently wrong under misspecification.

### Prior sensitivity

When data are weak in a coefficient direction, the posterior remains strongly influenced by prior location, scale, and dependence. “Weakly informative” is context-dependent, not a universal setting.

### Computational scaling

Dense coefficient-space conjugate inference forms and factorizes a $p\times p$ precision matrix, with typical costs $O(np^2+p^3)$ and $O(p^2)$ storage. Large or structured problems require alternative formulations or approximations.

### Approximation error

MCMC, variational inference, Laplace approximations, and other procedures target the posterior differently. Optimization or sampling success does not guarantee the approximation represents tails, modes, or correlations accurately.

### Prior–likelihood confounding

Different combinations of prior scale and noise variance can imply similar shrinkage. Hyperparameter learning may be weakly identified, especially in small samples.

### Interpretation remains conditional

A posterior for $\beta_j$ quantifies uncertainty inside the regression model; it does not turn an observational coefficient into a causal effect.

## Failure Modes

### Improper posterior

Improper or excessively diffuse priors combined with rank deficiency or poorly identified variance parameters can fail to produce a normalizable posterior.

### Prior–data conflict

A strongly concentrated prior far from the likelihood can dominate results or create computational pathologies. Prior predictive checks should reveal implausible implications before fitting.

### Gaussian error misspecification

Outliers, heteroscedasticity, censoring, or dependence can make posterior means and credible intervals misleading. A more suitable likelihood is a model change, not an inference tweak.

### MCMC nonconvergence

Divergences, poor mixing, low effective sample size, or unrecognized multimodality make Monte Carlo summaries unreliable even when chains finish.

### Variational underdispersion

Restricted variational families often underestimate posterior dependence and tail uncertainty. A high evidence lower bound does not prove calibrated uncertainty.

### Data-dependent prior tuning without accounting

Repeatedly choosing priors after viewing outcomes and then presenting the final posterior as prespecified hides researcher degrees of freedom and can overstate certainty.

### Predictive shift

Posterior uncertainty generally describes uncertainty under the training distribution. It does not automatically include future distribution shift, measurement changes, or unknown model classes.

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

