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

## Notation

This note introduces no special mathematical symbols. Code identifiers, class names, routine names, and hardware names are literal technical names rather than algebraic variables.

## Intuition

Hardware is the physical machinery that performs arithmetic. It can make the same numerical procedure faster or slower, but changing the machine does not change the definition of the model, objective, or solver.

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

## References

1. [Intel 64 and IA-32 Architectures Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html). Official manuals — instruction sets, vector execution, memory, caches, and concurrency.
2. [OpenBLAS documentation](https://www.openmathlib.org/OpenBLAS/docs/). Open implementation documentation — CPU kernels, threading, architecture dispatch, and numerical-library builds.
3. [RISC-V unprivileged ISA specification](https://docs.riscv.org/reference/isa/unpriv/unpriv-index.html). Open architecture specification — instruction execution, memory, floating point, vector operations, and concurrency.
4. [OpenMP API specification](https://www.openmp.org/specifications/). Open parallel-programming specification — threading, work sharing, synchronization, tasks, and accelerator offload.
