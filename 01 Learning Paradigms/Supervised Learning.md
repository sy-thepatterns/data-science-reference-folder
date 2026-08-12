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

Supervised learning estimates a mapping from inputs to known target values using labelled examples.

## Notation

| Symbol | Meaning |
|---|---|
| $\mathcal{D}$ | Labelled training dataset. |
| $n$ | Number of training examples. |
| $x_i$ | Input features for example $i$. |
| $y_i$ | Observed target for example $i$. |
| $f_\theta$ | Predictor with parameters $\theta$. |
| $\ell(f_\theta(x_i),y_i)$ | Loss on one example. |
| $\Omega(\theta)$ | Optional regularization penalty. |
| $\lambda$ | Nonnegative regularization strength. |
| $\hat\theta$ | Parameters estimated from training data. |
| $R(\theta)$ | Population or expected risk. |

## Intuition

A learner studies worked examples containing both questions and answers, then tries to answer new questions. Success depends not only on memorizing the examples but on learning a rule that continues to work when the new examples differ.

## Learning Signal

A target accompanies each training input. Targets may be categorical, continuous, ordered, structured, censored, noisy, delayed, or measured with unequal reliability.

## Main Settings

| Setting | Description |
|---|---|
| Batch | Train from a fixed labelled dataset. |
| Cost-sensitive | Assign different costs to different errors or examples. |
| Multitask | Learn several related targets jointly. |
| Weak supervision | Construct imperfect labels from rules, heuristics, or indirect sources. |
| Structured prediction | Predict sequences, trees, sets, or other dependent outputs. |

## Formal Setup

For

$$
\mathcal{D}=\{(x_i,y_i)\}_{i=1}^{n}
$$

empirical risk minimization chooses

$$
\hat\theta
=
\arg\min_\theta
\left[
\frac{1}{n}\sum_{i=1}^{n}\ell(f_\theta(x_i),y_i)
+
\lambda\Omega(\theta)
\right]
$$

The hypothesis class, loss, regularizer, optimizer, solver, implementation, and hardware are distinct choices.

## Typical Objectives and Strategies

- Empirical or regularized risk minimization.
- Likelihood or posterior predictive objectives for probabilistic prediction.
- Cost-sensitive risk aligned with decision consequences.
- Margin maximization.
- Calibration-aware or structured-output objectives.

## Data Structure and Splitting

The split must follow the unit expected to generalize: examples, people, entities, time periods, environments, or tasks. Preprocessing and model selection must use training data only. Any feedback-dependent collection process must be reproduced inside each training run rather than performed once using the full dataset.

## Main Tasks

- [[Regression]]
- [[Classification]]
- [[Ranking]]
- Forecasting and structured prediction

## Representative Algorithms

- [[Linear Regression]]
- [[Logistic Regression]]
- [[Decision Tree]]
- [[Support Vector Machine]]
- [[Neural Network]]

## Evaluation

- Use held-out examples that match the deployment unit: people, devices, groups, locations, or future times.
- Compare with simple, prior-system, and domain-relevant baselines.
- Report uncertainty across resamples or seeds and performance across meaningful subgroups.
- Evaluate calibration, threshold decisions, tail errors, and operational costs in addition to average discrimination.
- Fit preprocessing, feature selection, and hyperparameters inside training folds.
- Test distribution shift and inspect failure examples before deployment.

## Strengths

### Direct estimation of target risk

For iid labelled examples, empirical risk $\hat R_n(f)=n^{-1}\sum_i\ell(f(x_i),y_i)$ is an unbiased estimate of population risk for a fixed $f$. Labels therefore connect the learning objective directly to the predictive target.

### Consistency under stated conditions

With an appropriate hypothesis class, identifiable target, and regularity conditions, empirical-risk or likelihood estimators can converge toward the population optimum as $n\to\infty$. This provides a statistical foundation for learning curves and uncertainty analysis.

### Losses can identify specific functionals

Squared loss targets $\mathbb{E}[Y\mid X=x]$, absolute loss targets a conditional median, and log loss targets the full conditional class distribution. The loss therefore defines what statistical quantity the model estimates.

### Held-out evaluation

An independent test sample estimates generalization risk without reusing training outcomes. Its standard error decreases approximately as $1/\sqrt{n_{\mathrm{test}}}$, subject to dependence and metric assumptions.

### Bias–variance control

Model class, regularization, and sample size provide explicit ways to trade approximation bias against estimation variance. Learning curves can diagnose whether more data or a richer representation is likely to help.

### Calibrated decision support

Proper scoring rules reward truthful probability estimation in expectation. Estimated probabilities can then be combined with separate costs or utilities to make threshold decisions.

## Limitations and Failure Modes

### Generalization gap from finite samples

A flexible class can fit sampling noise. Bounds often scale schematically as

$$
R(f)-\hat R_n(f)
=
O\left(\sqrt{\frac{\text{complexity}+\log(1/\delta)}{n}}\right)
$$

showing why more capacity requires more independent data or stronger regularization.

### Irreducible label uncertainty

Even the Bayes predictor has nonzero error when $p(y\mid x)$ overlaps across outcomes or labels contain randomness. More training cannot remove this Bayes risk without better information or a redefined target.

### Measurement error and label noise

Noise in $y$ increases variance and can bias estimates when error depends on $x$ or the true label. In classification, asymmetric label noise changes the observed decision boundary rather than merely adding harmless variance.

### Dataset shift

Training minimizes risk under $p_{\mathrm{train}}(x,y)$, while deployment uses $p_{\mathrm{deploy}}(x,y)$. Empirical consistency on the first distribution gives no guarantee on the second without invariance or overlap assumptions.

### Leakage creates dependent evaluation

When test information affects features, tuning, or preprocessing, the test loss is conditionally optimized and no longer an unbiased estimate of new-data risk. Repeated test-set use has the same selection effect.

### Class imbalance and metric variance

Rare-class recall may be estimated from very few positive cases, giving wide uncertainty even in a large dataset. Accuracy weights errors by observed prevalence and can conceal clinically or operationally important minority failure.

### Observational prediction is not causal estimation

Minimizing predictive risk estimates associations in $p(y\mid x)$. Causal effects require assumptions about interventions, confounding, consistency, and positivity that ordinary supervised training does not supply.

### Correlated samples reduce information

Repeated entities, clusters, or temporal observations are not independent. Treating them as iid inflates the apparent sample size and understates validation and parameter uncertainty.

## Related Paradigms

- [[Semi-Supervised Learning]]
- [[Self-Supervised Learning]]
- [[Online Learning]]

## References

- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*.
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*.
