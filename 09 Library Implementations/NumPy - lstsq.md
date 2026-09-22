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

## Notation

| Symbol | Meaning |
|---|---|
| $A$ | Known $m\times n$ matrix defining the linear map. |
| $b$ | Known vector of $m$ observed values. |
| $x$ | Unknown vector of $n$ coefficients. |
| $x^\star$ | A coefficient vector that minimizes squared residual length. |
| $r=b-Ax$ | Residual vector. |
| $A^T$ | Transpose of $A$. |
| $\lVert\cdot\rVert_2$ | Euclidean norm. |
| $\nabla$ | Gradient with respect to the optimization variable. |
| $I$ | Identity matrix, when used. |

## Intuition

When an exact solution is impossible, least squares asks for the closest reachable point. Picture all vectors $Ax$ forming a flat sheet; it drops a perpendicular from $b$ to that sheet. The landing point is the fit and the perpendicular arrow is the residual.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Use projection geometry to prove the residual at an optimum is orthogonal to every column of $A$.
- Differentiate the squared residual norm to derive the normal equations $A^TAx=A^Tb$.
- Use the Hessian $2A^TA$ to prove convexity and full column rank to prove uniqueness.

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
