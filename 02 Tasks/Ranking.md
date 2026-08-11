---
type: task
name: Ranking
learning_paradigms:
  - "[[Supervised Learning]]"
algorithm_families:
  - "[[Linear Models]]"
  - "[[Tree-Based Methods]]"
  - "[[Neural Networks]]"
algorithms: []
metrics:
  - "[[Normalized Discounted Cumulative Gain]]"
  - "[[Mean Reciprocal Rank]]"
  - "[[Mean Average Precision]]"
datasets: []
applications:
  - "[[Search]]"
  - "[[Recommendation]]"
  - "[[Triage]]"
status: complete
tags:
  - ranking
---

# Ranking

## Problem Definition

Order candidate items by relevance, preference, risk, or utility for a query or context.

## Inputs

queries or contexts, candidate sets, and pointwise, pairwise, or listwise supervision.

## Outputs

scores $$s(q,d)$$ or an ordered candidate list.

## Formal Setup

Choose a scoring function whose induced permutation maximizes expected utility, often with position discount: $$U(\pi)=\sum_j w_j r_{\pi_j}$$.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Linear Models]]
- [[Tree-Based Methods]]
- [[Neural Networks]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Normalized Discounted Cumulative Gain]]
- [[Mean Reciprocal Rank]]
- [[Mean Average Precision]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Search]]
- [[Recommendation]]
- [[Triage]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Classification]]
- [[Recommendation]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
