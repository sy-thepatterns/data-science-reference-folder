---
type: task
name: Forecasting
learning_paradigms:
  - "[[Supervised Learning]]"
algorithm_families:
  - "[[Linear Models]]"
  - "[[Probabilistic Models]]"
  - "[[Neural Networks]]"
  - "[[Ensemble Methods]]"
algorithms: []
metrics:
  - "[[Mean Absolute Error]]"
  - "[[Root Mean Squared Error]]"
  - "[[Pinball Loss]]"
datasets: []
applications:
  - "[[Demand planning]]"
  - "[[Energy load]]"
  - "[[Capacity planning]]"
status: complete
tags:
  - forecasting
---

# Forecasting

## Problem Definition

Predict values or distributions at future horizons using information available at a forecast origin.

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

time-indexed history, covariates known at prediction time, and a forecast horizon.

## Outputs

point, quantile, interval, or probabilistic forecasts for future times.

## Formal Setup

At origin $t$ and horizon $h$, estimate $p(y_{t+h}\mid\mathcal{F}_t)$ without using information arriving after $t$.

## Subtasks

Common variants differ by supervision, output structure, horizon, whether uncertainty is required, and whether decisions are batch or online. These choices must be stated before comparing algorithms.

## Common Assumptions

Training and evaluation samples represent deployment after accounting for groups, time, exposure, censoring, and missingness. The chosen objective and metric must correspond to the intended estimand or decision cost.

## Algorithm Families

- [[Linear Models]]
- [[Probabilistic Models]]
- [[Neural Networks]]
- [[Ensemble Methods]]

Algorithm families describe modelling strategies. Loss functions, optimizers, numerical solvers, library implementations, and hardware backends are separate layers.

## Evaluation Metrics

- [[Mean Absolute Error]]
- [[Root Mean Squared Error]]
- [[Pinball Loss]]

Use deployment-realistic splits, uncertainty intervals, relevant subgroup slices, and a simple baseline. A metric is not the training algorithm even when the same mathematical function is used as a loss.

## Datasets

Dataset choice must document provenance, license, sampling unit, target construction, temporal coverage, known leakage risks, and whether repeated entities cross splits.

## Applications

- [[Demand planning]]
- [[Energy load]]
- [[Capacity planning]]

## Failure Modes

- Leakage from features, preprocessing, future data, duplicate entities, or label construction.
- Distribution shift and unsupported extrapolation.
- Optimizing a convenient proxy rather than deployment utility.
- Reporting aggregate performance without uncertainty, tail behaviour, or subgroup checks.
- Treating predictive association as a causal effect.

## Related Tasks

- [[Regression]]
- [[Time-Series Modelling]]
- [[Sequence Modelling]]

## References

- Hastie, Tibshirani, and Friedman, *The Elements of Statistical Learning*, 2009.
- Murphy, *Probabilistic Machine Learning: An Introduction*, 2022.
