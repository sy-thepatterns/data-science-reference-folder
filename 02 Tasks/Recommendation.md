---
type: task
name: Recommendation
learning_paradigms:
  - "[[Supervised Learning]]"
algorithm_families:
  - "[[Nearest-Neighbour Methods]]"
  - "[[Linear Models]]"
  - "[[Neural Networks]]"
  - "[[Reinforcement Learning Algorithms]]"
algorithms: []
metrics:
  - "[[Normalized Discounted Cumulative Gain]]"
  - "[[Recall at K]]"
  - "[[Mean Average Precision]]"
datasets: []
applications:
  - "[[Media discovery]]"
  - "[[Retail]]"
  - "[[Personalization]]"
status: complete
tags:
  - recommendation
---

# Recommendation

## Problem Definition

Select or rank items for a user or context to improve a stated utility subject to eligibility, diversity, and exposure constraints.

## Inputs

user–item interactions, item/user features, context, and candidate sets.

## Outputs

scores, rankings, or policies over eligible items.

## Formal Setup

Estimate utility $$u(i\mid u,c)$$ and choose a slate $$S$$ maximizing expected value under constraints; logged feedback is affected by the previous exposure policy.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Nearest-Neighbour Methods]]
- [[Linear Models]]
- [[Neural Networks]]
- [[Reinforcement Learning Algorithms]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Normalized Discounted Cumulative Gain]]
- [[Recall at K]]
- [[Mean Average Precision]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Media discovery]]
- [[Retail]]
- [[Personalization]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Ranking]]
- [[Forecasting]]
- [[Representation Learning]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
