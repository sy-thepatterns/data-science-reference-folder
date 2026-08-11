---
type: algorithm-family
name: Generative Models
parent_family: []
tasks: []
members:
  - "[[Variational Autoencoder]]"
  - "[[Generative Adversarial Network]]"
  - "[[Normalizing Flow]]"
  - "[[Diffusion Model]]"
related:
  - "[[Probabilistic Models]]"
  - "[[Neural Networks]]"
status: complete
tags:
  - generative-models
---

# Generative Models

## Definition

Model a data-generating distribution or an implicit sampling process.

## Unifying Principle

Members share the modelling structure below but may use different objectives, estimators, numerical solvers, implementations, and hardware. Those layers must be documented separately.

## Shared Mathematical Structure

Latent-variable models use $$p_\theta(x)=\int p_\theta(x\mid z)p(z)\,dz$$; autoregressive, adversarial, flow, and diffusion models use different objectives and sampling procedures.

## Typical Tasks

Members may address [[Classification]], [[Regression]], [[Representation Learning]], [[Generative Modelling]], or structured decision tasks. Applicability depends on the individual member and objective.

## Members

- [[Variational Autoencoder]]
- [[Generative Adversarial Network]]
- [[Normalizing Flow]]
- [[Diffusion Model]]

## Family Tree

```text
Generative Models
├── Variational Autoencoder
├── Generative Adversarial Network
└── Diffusion Model
```

## Shared Strengths

Flexible sampling, missing-data reasoning, simulation, and representation learning.

## Shared Limitations

Evaluation is difficult; training and sampling can be expensive, likelihood may not match perceptual quality, and generated data can reproduce bias.

## Comparison Table

| Member | Distinguishing choice | Training | Inference |
|---|---|---|---|
| [[Variational Autoencoder]] | Canonical member | Member- and solver-dependent | Model-dependent |
| [[Diffusion Model]] | Specialized variant | Data-, precision-, and implementation-dependent | Model-dependent |

Complexity must be stated on the member note with symbols, sparsity, solver, stopping rule, and backend; there is no valid universal family complexity.

## Related Families

- [[Probabilistic Models]]
- [[Neural Networks]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
