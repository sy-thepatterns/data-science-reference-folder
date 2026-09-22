---
type: implementation
name: PyTorch - Linear Regression
algorithm:
  - "[[Linear Regression]]"
library:
  - "[[PyTorch]]"
backend:
  - ATen
  - BLAS
  - CUDA libraries
hardware:
  - "[[CPU]]"
  - "[[GPU]]"
status: reviewed
tags:
  - autograd
  - gpu-friendly
  - iterative
---

# PyTorch - Linear Regression

## Important Distinction

A `torch.nn.Linear` layer defines the affine model:

$$
\hat{y}=XW^{T}+b
$$

It does not, by itself, perform ordinary-least-squares fitting. Training requires a loss and optimization procedure.

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
import torch
from torch import nn

model = nn.Linear(in_features=p, out_features=1)
loss_fn = nn.MSELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=1e-2)

for _ in range(num_epochs):
    optimizer.zero_grad()
    predictions = model(X)
    loss = loss_fn(predictions, y)
    loss.backward()
    optimizer.step()
```

## Execution Trace

```text
nn.Linear forward
    ↓
matrix multiplication and bias addition
    ↓
MSE loss
    ↓
autograd backward
    ↓
optimizer update
    ↓
ATen
    ↓
CPU kernels or CUDA libraries
    ↓
CPU or GPU
```

## Per-Epoch Complexity

For one target and full-batch training:

- Forward pass:

$$
O(np)
$$

- Loss computation:

$$
O(n)
$$

- Backward pass and gradient formation:

$$
O(np)
$$

- Parameter update:

$$
O(p)
$$

Thus the dominant per-epoch cost is:

$$
O(np)
$$

For $T$ epochs:

$$
O(Tnp)
$$

## Memory

The data require:

$$
O(np)
$$

The model requires:

$$
O(p)
$$

Autograd stores intermediate information needed for the backward pass. Mini-batching reduces active data memory to approximately:

$$
O(bp)
$$

for batch size $b$, excluding framework overhead.

## Differences from Direct OLS

- Iterative rather than a direct least-squares factorization.
- Requires learning rate and stopping choices.
- May not reach the exact least-squares solution in finite training.
- Naturally supports GPUs, mini-batches, custom losses, and larger neural architectures.
- Useful when linear regression is embedded in a differentiable model.
