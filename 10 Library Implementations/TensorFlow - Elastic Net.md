---
type: implementation
name: TensorFlow - Elastic Net
algorithm:
  - "[[Elastic Net]]"
library:
  - "[[TensorFlow]]"
status: reviewed
tags:
  - elastic-net
  - implementation
---

# TensorFlow - Elastic Net

## Implements

A custom differentiable implementation for [[Elastic Net]], whose defining objective is squared residual loss plus mixed L1 and L2 coefficient penalties.

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
| $\lambda$ | Nonnegative overall penalty strength. |
| $\alpha$ | Mixing fraction: $1$ gives the lasso part and $0$ gives the ridge part under this convention. |
| $\lVert\beta\rVert_1$ | Sum of absolute coefficient values. |
| $\lVert\beta\rVert_2^2$ | Sum of squared coefficient values. |
| $\partial$ | Subdifferential used for the nondifferentiable absolute-value term. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Elastic net uses two kinds of pull on each coefficient. One can snap weak coefficients to exactly zero; the other smoothly keeps large or highly correlated coefficients from becoming unstable. The mixing value decides how much of each behaviour you want.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Recover lasso and ridge by substituting $\alpha=1$ and $\alpha=0$ into the objective.
- Prove convexity because squared loss, the $L_1$ norm, and the squared $L_2$ norm are convex and have nonnegative weights.
- Derive a coordinate update by combining soft thresholding with the extra quadratic shrinkage term.

## Public API

```python
tf.keras.layers.Dense(1, kernel_regularizer=tf.keras.regularizers.L1L2(l1, l2))
```

## API and Fitting Route

| Property | Value |
|---|---|
| Primary API | `tf.keras.regularizers.L1L2` |
| Fitting style | Keras iterative training |
| Core solver route | Chosen Keras optimizer |
| Statistical inference | None automatic |
| Sparse support | Not guaranteed exact-zero |
| GPU support | Yes |

## Objective Mapping

The intended mathematical target is squared residual loss plus mixed L1 and L2 coefficient penalties. Constants, reductions, intercept treatment, sample weighting, and regularization parameterization can differ between this route and another package.

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
\text{O(Tnp)}
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

Regularizer reductions and optimizer behaviour must be reconciled with the chosen mathematical parameterization.

## Hardware and Backend

GPU availability in the table refers to this route, not merely to whether some dependency can run on a GPU. Low-level kernels and hardware remain separate notes from the estimator or custom implementation.

## Best Use

Use this route when its API level, solver behaviour, inference outputs, ecosystem integration, and hardware support match the project. Compare all six routes in [[Elastic Net Implementation Comparison]] before treating package choice as interchangeable.

## References

- Official TensorFlow documentation for the named API or building blocks.
- [[Elastic Net]]
- [[Elastic Net Implementation Comparison]]
