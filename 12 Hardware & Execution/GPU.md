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

## References

1. [NVIDIA CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/). Official documentation — GPU execution model, memory hierarchy, kernels, streams, and synchronization.
2. [PyTorch CUDA semantics](https://docs.pytorch.org/docs/stable/notes/cuda.html). Official documentation — device placement, asynchronous execution, streams, memory, and precision.
3. [AMD ROCm programming guides](https://rocm.docs.amd.com/en/latest/how-to/programming-guides.html). Official documentation — AMD GPU programming, HIP kernels, memory, streams, and performance.
4. [Khronos OpenCL Guide](https://github.com/KhronosGroup/OpenCL-Guide). Open standard guide — portable heterogeneous execution, devices, command queues, kernels, and memory.
