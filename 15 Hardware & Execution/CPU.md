---
type: hardware
name: CPU
category: General-purpose processor
used_by:
  - "[[Linear Regression]]"
  - "[[LAPACK]]"
related:
  - "[[GPU]]"
status: developing
tags:
  - cpu
---

# CPU

## Role in Machine Learning

CPUs execute general-purpose control logic and many dense or sparse numerical routines. Tabular linear regression commonly runs efficiently on a CPU through optimized BLAS and LAPACK libraries.

## Execution Path

```text
Python estimator
    ↓
NumPy or SciPy
    ↓
BLAS / LAPACK
    ↓
compiled kernels
    ↓
CPU vector instructions, cache, and threads
```

## Important Constraints

- Memory bandwidth
- Cache locality
- Threading overhead
- Matrix shape
- Numerical precision
- BLAS implementation

## Appropriate Workloads

- Small and medium tabular problems
- Sparse linear algebra
- Low-latency inference
- Numerically robust factorization through mature libraries
