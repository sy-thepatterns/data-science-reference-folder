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

## References

1. [NumPy linear algebra documentation](https://numpy.org/doc/stable/reference/routines.linalg.html). Package documentation — array-based linear algebra, solves, least squares, SVD, and pseudoinverses.
2. [NumPy source — `numpy.linalg`](https://github.com/numpy/numpy/tree/main/numpy/linalg). Official source — Python wrappers, gufunc dispatch, LAPACK calls, dtypes, and result shaping.
3. [Harris et al. — “Array Programming with NumPy”](https://www.nature.com/articles/s41586-020-2649-2.pdf). Open implementation paper — array model, vectorization, interoperability, compiled kernels, and ecosystem.
4. [Hastie, Tibshirani, and Friedman — *The Elements of Statistical Learning*](https://hastie.su.domains/Papers/ESLII.pdf). Open textbook — statistical learning theory, supervised and unsupervised methods, regularization, kernels, trees, ensembles, and model assessment.
