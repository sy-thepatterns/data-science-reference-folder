---
type: task
name: Classification
learning_paradigms:
  - "[[Supervised Learning]]"
algorithm_families:
  - "[[Linear Models]]"
  - "[[Tree-Based Methods]]"
  - "[[Kernel Methods]]"
  - "[[Nearest-Neighbour Methods]]"
  - "[[Neural Networks]]"
  - "[[Probabilistic Models]]"
algorithms: []
metrics:
  - "[[Accuracy]]"
  - "[[Precision]]"
  - "[[Recall]]"
  - "[[F1 Score]]"
  - "[[Log Loss]]"
datasets: []
applications:
  - "[[Diagnosis support]]"
  - "[[Document routing]]"
  - "[[Image recognition]]"
status: complete
tags:
  - classification
---

# Classification

## Problem Definition

Assign inputs to discrete classes or estimate probabilities over those classes.

## Inputs

labelled pairs $$(x_i,y_i)$$ with $$y_i\in\{1,\ldots,k\}$$.

## Outputs

hard labels, class scores, or probabilities $$p(y=c\mid x)$$.

## Formal Setup

The Bayes classifier chooses $$g^{\star}(x)\in\arg\max_c P(Y=c\mid X=x)$$ under equal misclassification costs.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Linear Models]]
- [[Tree-Based Methods]]
- [[Kernel Methods]]
- [[Nearest-Neighbour Methods]]
- [[Neural Networks]]
- [[Probabilistic Models]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Accuracy]]
- [[Precision]]
- [[Recall]]
- [[F1 Score]]
- [[Log Loss]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Diagnosis support]]
- [[Document routing]]
- [[Image recognition]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Regression]]
- [[Ranking]]
- [[Anomaly Detection]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
