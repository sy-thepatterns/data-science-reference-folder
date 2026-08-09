---
type: algorithm-family
name: Bayesian Methods
members:
  - "[[Bayesian Linear Regression]]"
  - "[[Gaussian Process]]"
status: developing
tags:
  - probabilistic
  - bayesian
---

# Bayesian Methods

## Definition

Bayesian methods combine a probability model and prior distribution with observed data to obtain a posterior distribution over unknown quantities.

## Unifying Principle

Bayes' theorem gives:

$$
p(\theta\mid\mathcal{\{D\}})
=
\frac{
p(\mathcal{\{D\}}\mid\theta)p(\theta)
}{
p(\mathcal{\{D\}})
}
$$

where:

$$
p(\mathcal{\{D\}})
=
\int
p(\mathcal{\{D\}}\mid\theta)p(\theta)
\,d\theta
$$

Predictions integrate over posterior uncertainty:

$$
p(y_{\star}\mid x_{\star},\mathcal{\{D\}})
=
\int
p(y_{\star}\mid x_{\star},\theta)
p(\theta\mid\mathcal{\{D\}})
\,d\theta
$$

## Shared Mathematical Structure

- A likelihood describes observations conditional on parameters.
- A prior describes uncertainty before conditioning on the current data.
- A posterior updates uncertainty after observing data.
- A posterior predictive distribution integrates parameter uncertainty.
- A decision rule may combine the posterior with a separate utility or loss.

Exact conjugate algebra, Markov chain Monte Carlo, variational inference, and Laplace approximation are different inference procedures. They are not the Bayesian model itself.

## Members

- [[Bayesian Linear Regression]]
- [[Gaussian Process]]

## Shared Strengths

- Explicit representation of uncertainty.
- Coherent incorporation of prior information.
- Posterior predictive distributions rather than only point estimates.
- Natural hierarchical and sequential modelling.

## Shared Limitations

- Sensitivity to likelihood and prior assumptions.
- Exact inference may be unavailable or expensive.
- Approximate inference requires separate validation.
- Posterior certainty can be misleading under misspecification.

## Diagnostics

- Prior predictive checks.
- Posterior predictive checks.
- Prior sensitivity analysis.
- Calibration checks.
- Sampling or approximation diagnostics appropriate to the inference procedure.

## Related Families

- [[Probabilistic Models]]
- [[Linear Models]]


