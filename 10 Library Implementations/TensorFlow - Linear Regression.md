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

For $$T$$ full-batch epochs and one output, the dominant work is:

$$
O(Tnp)
$$

With mini-batches, the total arithmetic per complete pass remains of the same order, although memory use and execution behaviour differ.

## Difference from Direct OLS

This is an iterative optimization approach. It is appropriate when integration with TensorFlow models, automatic differentiation, streaming batches, or accelerators matters more than obtaining a direct dense least-squares solution.
