---
type: learning-paradigm
name: Transfer Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
  - "[[Representation Learning]]"
algorithms:
  - "[[Fine-Tuning]]"
  - "[[Feature Extraction]]"
  - "[[Domain Adaptation]]"
architectures: []
related:
  - "[[Meta-Learning]]"
  - "[[Self-Supervised Learning]]"
  - "[[Supervised Learning]]"
status: complete
tags:
  - transfer-learning
---

# Transfer Learning

## Definition

Transfer learning reuses knowledge learned in a source setting to improve learning in a related target setting.

## Learning Signal

Source data, parameters, or representations plus target-domain feedback, which may be labelled or unlabelled.

## Data Structure

Training data must be partitioned according to the unit that will generalize: examples, users, time, environments, or tasks. Any statistics learned during preprocessing belong inside the training split.

## Formal Setup

Given source domain/task $$(\mathcal{D}_S,\mathcal{T}_S)$$ and target $$(\mathcal{D}_T,\mathcal{T}_T)$$, seek lower target risk by initializing, constraining, or augmenting target learning with source knowledge.

## Typical Objective

Feature reuse, fine-tuning, parameter regularization, and domain adaptation make different assumptions about what is shared.

## Main Tasks

- [[Classification]]
- [[Regression]]
- [[Representation Learning]]

## Representative Algorithms

- [[Fine-Tuning]]
- [[Feature Extraction]]
- [[Domain Adaptation]]

## Evaluation

Use held-out units that reproduce deployment conditions. Compare against a simple baseline, report uncertainty across splits or seeds, and measure data, label, compute, and latency costs when relevant.

## Strengths

Reduces target data and compute requirements and can import broadly useful representations.

## Limitations

Negative transfer, source bias, forgetting, incompatible label spaces, hidden data leakage, and costly target validation.

## Related Paradigms

- [[Meta-Learning]]
- [[Self-Supervised Learning]]
- [[Supervised Learning]]

## References

- Bishop, *Pattern Recognition and Machine Learning*, 2006.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
