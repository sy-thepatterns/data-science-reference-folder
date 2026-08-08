---
type: implementation
name: NumPy - lstsq
algorithm:
  - "[[Least Squares]]"
library:
  - "[[NumPy]]"
backend:
  - "[[LAPACK]]"
hardware:
  - "[[CPU]]"
version: "NumPy stable documentation inspected on 2026-08-06"
status: reviewed
tags:
  - linear-algebra
  - cpu
---

# NumPy - lstsq

## Public API

```python
import numpy as np

beta, residuals, rank, singular_values = np.linalg.lstsq(
    X,
    y,
    rcond=None,
)
```

## Mathematical Problem

The function returns a vector or matrix that minimizes:

$$
\lVert y-X\beta\rVert_2
$$

for underdetermined, well-determined, or overdetermined systems.

## Execution Layer

NumPy linear-algebra operations rely on BLAS and LAPACK implementations linked into the installed NumPy build.

```text
numpy.linalg.lstsq
    ↓
NumPy linear-algebra wrapper
    ↓
LAPACK routine
    ↓
BLAS kernels
    ↓
CPU
```

## Complexity

The dense factorization is shape-dependent, commonly summarized as:

$$
O\left(
\min\left(np^2,n^2p\right)
\right)
$$

for a dense SVD-style least-squares route.

## Difference from a Regression Estimator

`numpy.linalg.lstsq`:

- does not provide an estimator object;
- does not automatically manage feature names or pipelines;
- does not automatically add an intercept;
- does not provide statistical inference;
- directly solves the supplied matrix equation.

To fit an intercept, explicitly augment or center the design.
