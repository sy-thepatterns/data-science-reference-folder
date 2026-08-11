---
type: learning-paradigm
name: Supervised Learning
tasks:
  - "[[Regression]]"
  - "[[Classification]]"
  - "[[Ranking]]"
algorithms:
  - "[[Linear Regression]]"
  - "[[Logistic Regression]]"
  - "[[Decision Tree]]"
  - "[[Support Vector Machine]]"
  - "[[Neural Network]]"
architectures: []
related:
  - "[[Semi-Supervised Learning]]"
  - "[[Self-Supervised Learning]]"
  - "[[Online Learning]]"
status: complete
tags:
  - supervised
---

# Supervised Learning

## Definition

Supervised learning estimates a mapping from inputs to known targets using labelled examples.

## Learning Signal

A target $$y_i$$ accompanies each training input $$x_i$$; labels may be noisy, censored, delayed, or costly.

## Data Structure

Training data must be partitioned according to the unit that will generalize: examples, users, time, environments, or tasks. Any statistics learned during preprocessing belong inside the training split.

## Formal Setup

For $$\mathcal{D}=\{(x_i,y_i)\}_{i=1}^{n}$$, choose $$f_\theta$$ from a hypothesis class using held-out model selection and empirical risk minimization.

## Typical Objective

$$\hat{\theta}=\arg\min_\theta \frac{1}{n}\sum_{i=1}^{n}\ell(f_\theta(x_i),y_i)+\lambda\Omega(\theta)$$. The loss, regularizer, optimizer, implementation, and hardware remain separate notes.

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

Use held-out units that reproduce deployment conditions. Compare against a simple baseline, report uncertainty across splits or seeds, and measure data, label, compute, and latency costs when relevant.

## Strengths

Clear prediction target, direct validation, and a broad range of mature methods.

## Limitations

Label cost and bias, leakage, overfitting, distribution shift, confounding, and mismatch between training loss and deployment cost.

## Related Paradigms

- [[Semi-Supervised Learning]]
- [[Self-Supervised Learning]]
- [[Online Learning]]

## References

- Bishop, *Pattern Recognition and Machine Learning*, 2006.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
