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

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

A factorization such as $$p(x,z)=p(z)p(x\mid z)$$ defines assumptions; inference computes conditionals or expectations, while learning estimates parameters.

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
