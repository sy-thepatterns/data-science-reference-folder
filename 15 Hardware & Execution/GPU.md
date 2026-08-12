---
type: hardware
name: GPU
category: Massively parallel accelerator
used_by:
  - "[[PyTorch]]"
  - "[[TensorFlow]]"
related:
  - "[[CPU]]"
status: developing
tags:
  - gpu
  - parallelizable
---

# GPU

## Notation

This note introduces no special mathematical symbols. Code identifiers, class names, routine names, and hardware names are literal technical names rather than algebraic variables.

## Intuition

Hardware is the physical machinery that performs arithmetic. It can make the same numerical procedure faster or slower, but changing the machine does not change the definition of the model, objective, or solver.

## Role in Machine Learning

GPUs accelerate large, regular, parallel tensor operations. They are especially useful for large batches, high-dimensional dense matrices, and deep-learning workloads.

## Execution Path

```text
PyTorch or TensorFlow
    ↓
framework operator
    ↓
CUDA or another device runtime
    ↓
vendor numerical library or custom kernel
    ↓
GPU
```

## Linear Regression

Iterative gradient-based linear regression can run on a GPU through tensor frameworks. A direct least-squares solve may also have GPU implementations, but for small tabular datasets device-transfer and launch overhead can dominate.

## Bottlenecks

- Host-to-device transfer
- Kernel launch overhead
- Device memory
- Memory bandwidth
- Precision requirements
- Insufficient parallel work
