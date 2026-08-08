---
type: learning-paradigm
name: Supervised Learning
tasks:
  - "[[Regression]]"
  - "[[Classification]]"
algorithms:
  - "[[Linear Regression]]"
related:
  - "[[Semi-Supervised Learning]]"
  - "[[Self-Supervised Learning]]"
status: developing
tags:
  - supervised
---

# Supervised Learning

## Definition

Supervised learning estimates a mapping from inputs to known target values using labelled examples.

## Formal Setup

A labelled dataset is:

$$
\mathcal{D} = \{\,(x_i,y_i)\,\}_{i=1}^{n}
$$

where each input $$x_i$$ is paired with a target $$y_i$$.

The learner selects a function $$f_\theta$$ from a hypothesis class by minimizing an empirical objective:

$$
\hat{\theta}
=
\arg\min_{\theta}
\frac{1}{n}
\sum_{i=1}^{n}
\ell\left(f_\theta(x_i),y_i\right)
$$

## Main Tasks

- [[Regression]]
- [[Classification]]
- [[Ranking]]

## Representative Algorithms

- [[Linear Regression]]
- [[Logistic Regression]]
- [[Decision Tree]]
- [[Support Vector Machine]]
- [[Neural Network]]

## Evaluation

Evaluation should use held-out data or a resampling procedure that estimates generalization beyond the training examples.
