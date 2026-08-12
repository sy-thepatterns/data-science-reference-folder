---
type: algorithm-family
name: Probabilistic Models
parent_family: []
tasks: []
members:
  - "[[Naive Bayes]]"
  - "[[Gaussian Mixture Model]]"
  - "[[Hidden Markov Model]]"
  - "[[Conditional Random Field]]"
related:
  - "[[Bayesian Methods]]"
  - "[[Generative Models]]"
status: complete
tags:
  - probabilistic-models
---

# Probabilistic Models

## Definition

Specify joint or conditional probability distributions over observed and latent variables.

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
| $P$ | Probability measure or event probability. |
| $p_\theta$ | Probability mass or density indexed by parameters $\theta$. |
| $\mathcal{D}$ | Observed dataset. |
| $\mathbb{E}$ | Expected value under the stated distribution. |
| $H$ | Entropy, a measure of uncertainty in a probability distribution. |
| $\arg\min,\arg\max$ | Input value or set of values attaining a minimum or maximum. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Instead of claiming one outcome must happen, a probabilistic model spreads belief across possible outcomes. Learning changes that spread using observed data. The exact shape of the spread matters because two models can make the same average prediction while expressing very different uncertainty.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Check normalization and nonnegativity for the stated probability model.
- Derive the objective from its likelihood or expected-risk definition rather than treating it as an unexplained formula.
- Use the law of total probability or expectation to connect latent, conditional, and marginal quantities.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

A factorization such as $p(x,z)=p(z)p(x\mid z)$ defines assumptions; inference computes conditionals or expectations, while learning estimates parameters.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Naive Bayes]]
- [[Gaussian Mixture Model]]
- [[Hidden Markov Model]]
- [[Conditional Random Field]]

## Family Tree

```text
Probabilistic Models
├── Naive Bayes
├── Gaussian Mixture Model
└── Conditional Random Field
```

## Shared Strengths

Explicit uncertainty, missing-data handling, interpretable assumptions, and coherent composition.

## Shared Limitations

Misspecification, identifiability, intractable inference, local optima, and probabilities that may be poorly calibrated under shift.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Naive Bayes]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Conditional Random Field]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Bayesian Methods]]
- [[Generative Models]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
