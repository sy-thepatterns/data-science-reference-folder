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
| $P$ | Probability measure or event probability. |
| $p_\theta$ | Probability mass or density indexed by parameters $\theta$. |
| $\mathcal{D}$ | Observed dataset. |
| $\mathbb{E}$ | Expected value under the stated distribution. |
| $H$ | Entropy, a measure of uncertainty in a probability distribution. |
| $\arg\min,\arg\max$ | Input value or set of values attaining a minimum or maximum. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Instead of claiming one outcome must happen, a probabilistic model spreads belief across possible outcomes. Learning changes that spread using observed data. The exact shape of the spread matters because two models can make the same average prediction while expressing very different uncertainty.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Check normalization and nonnegativity for the stated probability model.
- Derive the objective from its likelihood or expected-risk definition rather than treating it as an unexplained formula.
- Use the law of total probability or expectation to connect latent, conditional, and marginal quantities.

## Inputs

observations, optional conditions, and sometimes latent variables.

## Outputs

samples, conditional samples, likelihoods, or latent representations.

## Formal Setup

Fit $p_\theta(x)$ or $p_\theta(x\mid c)$ by likelihood, variational bounds, adversarial objectives, score matching, or related criteria.

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
