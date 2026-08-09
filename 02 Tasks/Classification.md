---
type: task
name: Classification
learning_paradigms:
  - "[[Supervised Learning]]"
algorithm_families:
  - "[[Linear Models]]"
  - "[[Tree-Based Methods]]"
  - "[[Kernel Methods]]"
  - "[[Neural Networks]]"
algorithms:
  - "[[Logistic Regression]]"
metrics: []
datasets: []
applications: []
status: developing
tags:
  - classification
---

# Classification

## Problem Definition

Classification assigns an input to one or more discrete classes or estimates probabilities over those classes. The task is distinct from the algorithm used to solve it.

## Inputs

A labelled dataset:

$$
\mathcal{\{D\}}
=
\{\,(x_i,y_i)\,\}_{i=1}^{n}
$$

with:

$$
x_i\in\mathcal{X}
$$

and, for single-label classification:

$$
y_i\in\{1,\ldots,k\}
$$

## Outputs

A hard classifier:

$$
\hat{g}:\mathcal{X}\rightarrow\{1,\ldots,k\}
$$

or class probabilities:

$$
\hat{p}(y=c\mid x)
$$

for each class:

$$
c\in\{1,\ldots,k\}
$$

## Formal Setup

With zero-one loss, the Bayes-optimal classifier predicts a class with greatest conditional probability:

$$
g^{\star}(x)
\in
\arg\max_c
P(Y=c\mid X=x)
$$

Real systems may instead choose actions that minimize expected cost under asymmetric errors. Probability estimation and the downstream decision rule should therefore be evaluated separately.

## Algorithm Families

- [[Linear Models]]
- [[Tree-Based Methods]]
- [[Kernel Methods]]
- [[Nearest-Neighbour Methods]]
- [[Neural Networks]]
- [[Probabilistic Models]]

## Representative Algorithm

- [[Logistic Regression]]

## Evaluation

Appropriate evaluation can include discrimination, calibration, threshold-dependent error, subgroup performance, and deployment cost. Accuracy alone is often insufficient under class imbalance.

## Common Failure Modes

- Label leakage or inconsistent label definitions.
- Imbalanced classes concealed by aggregate accuracy.
- Threshold selection on the final test set.
- Dataset shift and changing class prevalence.
- Poor probability calibration.
- Unseen categories or classes at deployment.
- Treating predictive labels as causal conclusions.

## Related Tasks

- [[Regression]]
- [[Ranking]]
- [[Anomaly Detection]]
- [[Multi-Label Classification]]


