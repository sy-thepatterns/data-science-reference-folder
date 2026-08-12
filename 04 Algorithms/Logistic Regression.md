---
type: algorithm
name: Logistic Regression
aliases:
  - Logit Model
learning_paradigm:
  - "[[Supervised Learning]]"
task:
  - "[[Classification]]"
family:
  - "[[Linear Models]]"
foundations:
  - Probability
  - Maximum Likelihood Estimation
objective:
  - Conditional log-likelihood
loss:
  - Binary Cross-Entropy
optimization:
  - Convex optimization
solvers:
  - Newton Method
  - Iteratively Reweighted Least Squares
  - Limited-Memory BFGS
  - Stochastic Gradient Descent
implementations:
  - "[[scikit-learn - LogisticRegression]]"
  - "[[statsmodels - Logit]]"
  - "[[NumPy - Logistic Regression]]"
  - "[[SciPy - Logistic Regression]]"
  - "[[PyTorch - Logistic Regression]]"
  - "[[TensorFlow - Logistic Regression]]"
related:
  - "[[Linear Regression]]"
  - Softmax Regression
status: reviewed
tags:
  - linear
  - parametric
  - frequentist
  - convex
  - differentiable
  - interpretable
  - discriminative
---

# Logistic Regression

## Overview

Logistic regression is a probabilistic classification algorithm despite its name. For binary classification, it models the conditional probability of one class by applying the logistic function to a linear score. The decision boundary is linear in the chosen feature representation.

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
| $\beta_0$ | Intercept: the prediction when all represented features are zero. |
| $\beta$ | Vector of $p$ coefficients; $\beta_j$ controls feature $j$ while other represented features are held fixed. |
| $\hat{\beta}$ | Estimated coefficient vector; a hat marks a quantity learned from data. |
| $X\beta$ | Vector of linear predictions before adding a separate intercept. |
| $\varepsilon$ | Unobserved error: the part of $y$ not represented by the linear mean model. |
| $r=y-X\beta$ | Residual vector: observed values minus fitted values. |
| $z_i$ | Linear score, or logit, for example $i$. |
| $\sigma(z)$ | Logistic function $1/(1+e^{-z})$, which turns any real score into a number between zero and one. |
| $p_i$ | Modelled probability that $y_i=1$. |
| $W$ | Diagonal matrix with entries $p_i(1-p_i)$ used in curvature calculations. |
| $\tau$ | Decision threshold used to turn a probability into a class action. |
| $L,\mathcal{L}$ | Likelihood and loss/objective, respectively; context distinguishes them. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Imagine a straight ruler that produces a score: moving along a feature changes that score by a fixed amount. The logistic curve bends the ruler's unlimited scores into probabilities between zero and one. A separate threshold then turns a probability into an action, so changing the threshold changes decisions without retraining the probability model.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Derive the log-odds identity by substituting $p=1/(1+e^{-z})$ and simplifying $\log(p/(1-p))$.
- Derive binary cross-entropy by taking the negative logarithm of the Bernoulli likelihood and using the logarithm-of-a-product rule.
- Prove convexity by showing $X^TWX/n$ is positive semidefinite because every diagonal entry of $W$ is nonnegative.

## Problem Definition

Given:

$$
X\in\mathbb{R}^{n\times p}
$$

and labels:

$$
y_i\in\{0,1\}
$$

estimate class probabilities:

$$
\hat{p}_i
=
P(y_i=1\mid x_i)
$$

and optional class predictions based on a decision threshold.

## Formal Definition

The linear score is:

$$
z_i
=
\beta_0+x_i^T\beta
$$

The logistic function is:

$$
\sigma(z)
=
\frac{1}{1+e^{-z}}
$$

The model is:

$$
P(y_i=1\mid x_i)
=
\sigma(z_i)
$$

Equivalently, the log-odds are linear:

$$
\log
\left(
\frac{P(y_i=1\mid x_i)}{1-P(y_i=1\mid x_i)}
\right)
=
\beta_0+x_i^T\beta
$$

## Likelihood and Loss

Assuming conditionally independent Bernoulli labels, the likelihood is:

$$
L(\beta_0,\beta)
=
\prod_{i=1}^{n}
p_i^{y_i}(1-p_i)^{1-y_i}
$$

The average negative log-likelihood, also called binary cross-entropy, is:

$$
\mathcal{L}(\beta_0,\beta)
=
-\frac{1}{n}
\sum_{i=1}^{n}
\left[
y_i\log p_i
+
(1-y_i)\log(1-p_i)
\right]
$$

For numerical stability, software commonly evaluates this objective from logits without first forming probabilities near zero or one.

## Gradient and Hessian

With an intercept included in the design matrix, the gradient is:

$$
\nabla_{\beta}\mathcal{L}
=
\frac{1}{n}X^T(p-y)
$$

The Hessian is:

$$
\nabla_{\beta}^2\mathcal{L}
=
\frac{1}{n}X^TWX
$$

where:

$$
W
=
\operatorname{diag}
\left(
p_i(1-p_i)
\right)
$$

Because the diagonal entries are nonnegative, the unregularized negative log-likelihood is convex.

## Decision Rule

For probability threshold:

$$
\tau\in(0,1)
$$

predict class one when:

$$
\hat{p}(x)\ge\tau
$$

The threshold is a decision-policy choice and is not part of probability estimation. It should reflect class prevalence and error costs rather than default blindly to one half.

## Statistical Properties

### Interpretation

Holding other features fixed, a one-unit increase in feature:

$$
x_j
$$

multiplies the modelled odds by:

$$
e^{\beta_j}
$$

This is a conditional association, not automatically a causal effect.

### Separation

If a hyperplane perfectly separates the classes, the unregularized maximum-likelihood coefficient norm can diverge and a finite optimum may not exist. Regularization or bias-reduced estimation can address this.

### Calibration

Logistic regression can provide well-calibrated probabilities when the conditional log-odds model is appropriate. Regularization, imbalance, sampling design, and distribution shift can affect calibration.

## Optimization and Solvers

There is no algebraic closed-form maximum-likelihood estimate. Newton-type methods, iteratively reweighted least squares, quasi-Newton methods, coordinate descent, and stochastic methods are numerical solvers for the objective.

Regularized logistic regression changes the objective by adding coefficient penalties. Multiclass softmax regression changes the probability model; one-versus-rest instead fits multiple binary models.

## Training Pseudocode

```text
INPUT:
    design matrix X
    binary labels y
    optional regularization
    selected numerical solver
    convergence tolerance

1. Validate labels and feature shapes.
2. Fit preprocessing on the training partition.
3. Initialize coefficients.
4. Compute logits and stable negative log-likelihood quantities.
5. Update coefficients with the selected solver.
6. Repeat until the stopping criterion is met.
7. Store coefficients, intercept, classes, and preprocessing metadata.

OUTPUT:
    fitted conditional probability model
```

## Prediction Pseudocode

```text
INPUT:
    new feature matrix X_new
    fitted coefficients and intercept
    optional decision threshold tau

1. Compute logits.
2. Apply the logistic function to obtain probabilities.
3. If labels are requested, compare probabilities with tau.

OUTPUT:
    class probabilities and optional class labels
```

## Complexity

Gradient evaluation for a dense dataset costs:

$$
O(np)
$$

For:

$$
T
$$

first-order iterations, a common high-level form is:

$$
O(Tnp)
$$

A dense Newton step may require forming a Hessian in:

$$
O(np^2)
$$

and solving a system in:

$$
O(p^3)
$$

These expressions depend on solver, sparsity, classes, conditioning, and stopping tolerance. Dense batch prediction costs:

$$
O(mp)
$$

## Hyperparameters

| Parameter | Mathematical effect | Typical behaviour |
|---|---|---|
| Regularization strength | Penalizes coefficient magnitude | Stronger regularization reduces variance |
| Penalty type | Selects squared, absolute, or mixed shrinkage | Changes sparsity and optimization |
| Class weights | Reweights observations in the objective | Changes the fitted conditional target under weighting |
| Solver tolerance | Controls numerical termination | Tighter values increase work |
| Decision threshold | Converts probability to action | Trades false positives against false negatives |

## Advantages

Logistic regression estimates a conditional Bernoulli distribution. Its statistical advantages should be judged through expected log loss, calibration, discrimination, coefficient sampling uncertainty, and decision risk—not accuracy alone.

### Valid probability range

The logistic link maps every real score to $(0,1)$:

$$
p(y=1\mid x)=\frac{1}{1+e^{-(\beta_0+x^T\beta)}}
$$

so the model produces a coherent Bernoulli probability rather than an unbounded class score.

### Convex negative log-likelihood

The Hessian is $X^TWX/n$ with diagonal $W_{ii}=p_i(1-p_i)\ge0$. The binary negative log-likelihood is therefore convex, and regularized variants remain convex for convex penalties.

### Interpretable log-odds

Holding other represented features fixed, increasing $x_j$ by one changes log-odds by $\beta_j$ and multiplies odds by $e^{\beta_j}$. This is a conditional association under the specified feature representation.

### Separate probability and decision layers

The model estimates $p(y=1\mid x)$; a threshold $\tau$ converts that estimate into an action. Thresholds can be changed for different error costs without refitting the probability model.

### Efficient prediction

Computing scores for $m$ dense rows costs $O(mp)$, followed by elementwise logistic transforms. Sparse feature matrices can make prediction proportional to nonzero entries.

### Strong extensibility

Regularization, basis expansions, interactions, sample weights, and multiclass generalizations preserve a clear relationship between score, likelihood, and decision rule.

### Useful probabilistic baseline

When log-odds are approximately linear, logistic regression often provides competitive discrimination and calibration with much less complexity than flexible nonlinear models.

### Maximum likelihood is asymptotically efficient under specification

When the logistic model is correctly specified and regularity conditions hold,

$$
\sqrt{n}(\hat\beta-\beta)
\xrightarrow{d}
\mathcal{N}\left(0,\mathcal{I}(\beta)^{-1}\right)
$$

where $\mathcal{I}(\beta)$ is Fisher information. This explains how sample size, class overlap, and feature geometry govern coefficient precision.

## Limitations

These limitations describe violations of identifiability, conditional log-odds specification, overlap, and sampling assumptions. A numerically converged optimizer cannot repair any of them.

### Linear log-odds assumption

The conditional probability may be nonlinear, but its log-odds must lie in the span of the supplied features. Missing interactions or nonlinear terms produce systematic probability error.

### No finite unregularized estimate under separation

If a hyperplane perfectly separates classes, increasing coefficient magnitude can drive likelihood toward its supremum without attaining a finite maximum. Regularization or a different inferential treatment is required.

### Coefficient instability

Collinearity makes several coefficient combinations produce similar logits. Standard errors grow and odds-ratio interpretations become sensitive even when class probabilities are stable.

### Likelihood targets probability, not every decision cost

Cross-entropy is a proper scoring rule, but deployment may care about asymmetric harm, capacity constraints, ranking, or utility. Those require separate threshold or policy analysis.

### Rare-event and small-sample bias

With few positive outcomes relative to parameters, maximum-likelihood coefficients can be biased and highly variable. Class weighting does not automatically repair probability calibration.

### Sensitivity to leverage and label error

Extreme feature values can have disproportionate effect, while confidently mislabeled examples contribute large loss and gradients.

### Association is not causality

An odds ratio describes the fitted conditional distribution, not the effect of intervening on a feature.

## Failure Modes

### Complete or quasi-complete separation

Coefficients diverge, standard errors become enormous, and an optimizer may stop only because of numerical limits. Convergence flags alone can conceal the statistical nonexistence of a finite MLE.

### Overflow and unstable log calculations

Naively computing $\log(1-p)$ after forming saturated probabilities can produce infinities. Stable implementations operate on logits with log-sum-exp identities.

### Threshold misuse

A default threshold of $0.5$ may be inappropriate under imbalance, asymmetric costs, or capacity limits. Selecting a threshold on the final test set leaks evaluation information.

### Prevalence and calibration shift

A model can retain ranking ability while probabilities become wrong after class prevalence or conditional relationships change. Calibration must be monitored separately from discrimination.

### Accuracy masking minority failure

A majority-class predictor can have high accuracy. Precision, recall, calibration, subgroup error, and decision utility reveal failures hidden by a single aggregate metric.

### Encoding and preprocessing leakage

Target encoding, imputation, scaling, and feature selection performed before splitting allow validation labels or population statistics to influence training.

### Unseen categories or unsupported regions

New categorical levels and feature combinations can make logits undefined in an implementation or unreliable as a model, even though the logistic function still returns a number.

## Diagnostics

- Check convergence and coefficient magnitude.
- Evaluate discrimination and calibration separately.
- Inspect confusion matrices across relevant thresholds.
- Detect separation and influential observations.
- Validate preprocessing and regularization without leakage.
- Compare nonlinear features or models when residual structure remains.

## Related Algorithms

- [[Linear Regression]] predicts an unbounded continuous response under a different likelihood and loss.
- Softmax regression generalizes the normalized probability model to mutually exclusive multiclass outcomes.
- Probit regression replaces the logistic link with the normal cumulative distribution function.

## Implementations

- [[scikit-learn - LogisticRegression]]
- [[statsmodels - Logit]]
- [[NumPy - Logistic Regression]]
- [[SciPy - Logistic Regression]]
- [[PyTorch - Logistic Regression]]
- [[TensorFlow - Logistic Regression]]
- [[Logistic Regression Implementation Comparison]]

## References

- Cox, D. R. (1958). *The Regression Analysis of Binary Sequences*.
- McCullagh, P., and Nelder, J. A. *Generalized Linear Models*.
- Hosmer, D. W., Lemeshow, S., and Sturdivant, R. X. *Applied Logistic Regression*.

