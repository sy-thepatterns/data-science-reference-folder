---
type: task
name: Sequence Modelling
learning_paradigms:
  - "[[Unsupervised Learning]]"
algorithm_families:
  - "[[Neural Networks]]"
  - "[[Probabilistic Models]]"
algorithms: []
metrics:
  - "[[Negative Log-Likelihood]]"
  - "[[Perplexity]]"
  - "[[Edit Distance]]"
datasets: []
applications:
  - "[[Language]]"
  - "[[Speech]]"
  - "[[Biological sequences]]"
status: complete
tags:
  - sequence-modelling
---

# Sequence Modelling

## Problem Definition

Model ordered observations whose position and dependence structure carry information.

## Notation

| Symbol | Meaning |
|---|---|
| $n$ | Number of observed examples or rows. |
| $p$ | Number of input features or columns. |
| $x_i$ | Feature vector for example $i$. |
| $y_i$ | Observed target or label for example $i$. |
| $X$ | Design matrix whose row $i$ is $x_i^T$; usually $X\in\mathbb{R}^{n\times p}$. |
| $y$ | Vector of all observed targets. |
| $\theta$ | Generic collection of parameters learned by a model. |
| $\ell$ | Loss assigned to a prediction and its observed target. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Read the formula as a recipe: identify what is observed, what must be learned, and what quantity says whether an answer is good. The notation compresses that recipe, but it does not turn the model into a solver or a software package.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Expand the stated objective from its definition and check that every term has compatible dimensions.
- Derive the first-order condition by differentiating or using a subgradient when the objective has a sharp corner.
- Check special cases and boundary values to confirm that the formula reduces to simpler known results.

## Inputs

variable-length sequences $(x_1,\ldots,x_T)$ with masks and optional targets.

## Outputs

sequence labels, per-step labels, next-token distributions, or generated sequences.

## Formal Setup

Autoregressive models factorize $p(x_{1:T})=\prod_{t=1}^{T}p(x_t\mid x_{<t})$; other factorizations support different inference patterns.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Neural Networks]]
- [[Probabilistic Models]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Negative Log-Likelihood]]
- [[Perplexity]]
- [[Edit Distance]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Language]]
- [[Speech]]
- [[Biological sequences]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Time-Series Modelling]]
- [[Forecasting]]
- [[Representation Learning]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
