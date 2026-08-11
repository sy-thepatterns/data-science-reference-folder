---
type: task
name: Generative Modelling
learning_paradigms:
  - "[[Unsupervised Learning]]"
algorithm_families:
  - "[[Generative Models]]"
  - "[[Probabilistic Models]]"
  - "[[Neural Networks]]"
algorithms: []
metrics:
  - "[[Negative Log-Likelihood]]"
  - "[[Fréchet Inception Distance]]"
  - "[[Precision and Recall for Distributions]]"
datasets: []
applications:
  - "[[Synthetic data]]"
  - "[[Content generation]]"
  - "[[Simulation]]"
status: complete
tags:
  - generative-modelling
---

# Generative Modelling

## Problem Definition

Learn a data-generating distribution or process from which new observations can be sampled or scored.

## Inputs

observations, optional conditions, and sometimes latent variables.

## Outputs

samples, conditional samples, likelihoods, or latent representations.

## Formal Setup

Fit $$p_\theta(x)$$ or $$p_\theta(x\mid c)$$ by likelihood, variational bounds, adversarial objectives, score matching, or related criteria.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Generative Models]]
- [[Probabilistic Models]]
- [[Neural Networks]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Negative Log-Likelihood]]
- [[Fréchet Inception Distance]]
- [[Precision and Recall for Distributions]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Synthetic data]]
- [[Content generation]]
- [[Simulation]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Density Estimation]]
- [[Representation Learning]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
