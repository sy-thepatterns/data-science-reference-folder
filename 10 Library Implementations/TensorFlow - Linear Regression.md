---
type: implementation
name: TensorFlow - Linear Regression
algorithm:
  - "[[Linear Regression]]"
library:
  - "[[TensorFlow]]"
hardware:
  - "[[CPU]]"
  - "[[GPU]]"
status: developing
tags:
  - autograd
  - gpu-friendly
  - iterative
---

# TensorFlow - Linear Regression

## Model

A dense layer with one output represents:

$$
\hat{y}=Xw+b
$$

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
| $\hat y$ | Vector of fitted or predicted responses. |
| $w_i$ | Optional nonnegative importance weight for example $i$. |
| $\mathbb{E}[\cdot]$ | Expected value under the stated probability model. |
| $\operatorname{Var}(\cdot)$ | Variance, measuring squared spread around an expectation. |
| $\lVert\cdot\rVert_2$ | Euclidean norm; its square sums squared entries. |
| $I$ | Identity matrix. |
| $\sigma^2$ | Error variance under a homoscedastic model. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Imagine fitting a flat sheet through a cloud of points. Each coefficient tilts the sheet in one feature direction. Least squares chooses the tilt that makes the combined vertical misses as small as possible, while the residuals are the arrows from the sheet to the observed points.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Expand $\lVert y-X\beta\rVert_2^2$, differentiate, and set the gradient to zero to obtain the normal equations.
- Use orthogonal projection to prove that the fitted vector lies in the column space of $X$ and the residual is perpendicular to that space.
- Prove convexity by showing the Hessian $2X^TX$ is positive semidefinite.

## Example

```python
import tensorflow as tf

model = tf.keras.Sequential([
    tf.keras.layers.Dense(1, input_shape=(p,))
])

model.compile(
    optimizer=tf.keras.optimizers.SGD(learning_rate=1e-2),
    loss="mse",
)

model.fit(X, y, epochs=num_epochs, batch_size=batch_size)
```

## Execution Trace

```text
Keras Dense layer
    ↓
TensorFlow tensor operations
    ↓
MSE objective
    ↓
automatic differentiation
    ↓
optimizer update
    ↓
device runtime
    ↓
CPU or GPU kernels
```

## Complexity

For $T$ full-batch epochs and one output, the dominant work is:

$$
O(Tnp)
$$

With mini-batches, the total arithmetic per complete pass remains of the same order, although memory use and execution behaviour differ.

## Difference from Direct OLS

This is an iterative optimization approach. It is appropriate when integration with TensorFlow models, automatic differentiation, streaming batches, or accelerators matters more than obtaining a direct dense least-squares solution.
