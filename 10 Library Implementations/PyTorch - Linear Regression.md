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

For $$T$$ epochs:

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

for batch size $$b$$, excluding framework overhead.

## Differences from Direct OLS

- Iterative rather than a direct least-squares factorization.
- Requires learning rate and stopping choices.
- May not reach the exact least-squares solution in finite training.
- Naturally supports GPUs, mini-batches, custom losses, and larger neural architectures.
- Useful when linear regression is embedded in a differentiable model.
