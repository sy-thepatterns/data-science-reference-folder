---
type: implementation
name: TensorFlow - Logistic Regression
algorithm:
  - "[[Logistic Regression]]"
library:
  - "[[TensorFlow]]"
status: reviewed
tags:
  - logistic
  - implementation
---

# TensorFlow - Logistic Regression

## Implements

A custom differentiable implementation for [[Logistic Regression]], whose defining objective is Bernoulli or multinomial negative log-likelihood, optionally regularized.

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

## Public API

```python
model = tf.keras.Sequential([tf.keras.layers.Dense(1)])
loss = tf.keras.losses.BinaryCrossentropy(from_logits=True)
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `tf.keras Dense plus BinaryCrossentropy` |
| Fitting style | Keras iterative training |
| Core solver route | Chosen Keras optimizer |
| Statistical inference | None automatic |
| Sparse support | Operation-specific |
| GPU support | Yes |

## Objective Mapping

The intended mathematical target is Bernoulli or multinomial negative log-likelihood, optionally regularized. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

## Execution Trace

```text
Public API or custom entry point
    ↓
shape validation and preprocessing
    ↓
Keras iterative training
    ↓
Chosen Keras optimizer
    ↓
TensorFlow numerical operations and dependencies
    ↓
available CPU or accelerator backend
```

## Complexity Variables

$$
n=\text{number of samples}
$$

$$
p=\text{number of features}
$$

$$
T=\text{number of solver iterations or training passes}
$$

$$
b=\text{mini-batch size}
$$

## Training Complexity

Representative time:

$$
\text{O(Tnp) for full passes}
$$

Representative additional or active space:

$$
\text{O(bp + p), plus optimizer state}
$$

These are route-level summaries, not universal bounds. Data shape, sparsity, active-set size, precision, convergence tolerance, line searches, batching, and linked numerical libraries can change actual cost.

## Prediction Complexity

For a dense fitted coefficient vector and:

$$
m=\text{number of prediction rows}
$$

prediction is dominated by a matrix-vector product:

$$
O(mp)
$$

with output storage:

$$
O(m)
$$

Sparse learned coefficients or sparse inputs can reduce arithmetic when the implementation exploits them.

## Numerical and Statistical Caveats

A sigmoid activation plus `from_logits=True` is incorrect; keep logits or set the loss convention consistently.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Logistic Regression Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official TensorFlow documentation for the named API or building blocks.
- [[Logistic Regression]]
- [[Logistic Regression Implementation Comparison]]
