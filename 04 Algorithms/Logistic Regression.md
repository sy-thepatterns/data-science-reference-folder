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

- Produces probabilistic predictions.
- Convex binary objective.
- Interpretable log-odds coefficients.
- Efficient training and prediction for many datasets.
- Strong baseline for classification.

## Limitations and Failure Modes

- Linear log-odds assumption in the selected features.
- Complete or quasi-complete separation.
- Sensitivity to influential leverage points.
- Multicollinearity can destabilize unregularized coefficients.
- Accuracy alone can conceal failure under imbalance.
- A fixed threshold may be inappropriate after prevalence shift.
- Probability estimates can be miscalibrated under misspecification or shift.

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

