---
type: implementation
name: SciPy - lstsq
algorithm:
  - "[[Least Squares]]"
library:
  - "[[SciPy]]"
backend:
  - "[[LAPACK]]"
numerical_methods:
  - "[[Singular Value Decomposition]]"
  - rank-revealing least squares
hardware:
  - "[[CPU]]"
version: "SciPy 1.18.0 documentation inspected on 2026-08-06"
status: reviewed
tags:
  - linear-algebra
  - cpu
---

# SciPy - lstsq

## Public API

```python
from scipy.linalg import lstsq

beta, residuals, rank, singular_values = lstsq(
    X,
    y,
    cond=None,
    overwrite_a=False,
    overwrite_b=False,
    check_finite=True,
    lapack_driver=None,
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

$$
\hat{\beta}
=
\arg\min_{\beta}
\lVert X\beta-y\rVert_2
$$

## Driver Choice

The official API exposes LAPACK driver choices:

- `gelsd`
- `gelsy`
- `gelss`

The documented default is `gelsd`. Drivers differ in speed, memory use, rank handling, and returned singular-value behaviour.

## Execution Trace

```text
scipy.linalg.lstsq
    ↓
argument checks and workspace preparation
    ↓
selected LAPACK driver
    ↓
factorization and least-squares solve
    ↓
BLAS operations
    ↓
CPU
```

## Complexity

Dense least-squares complexity is shape- and driver-dependent. A useful high-level bound for SVD-style dense solution is:

$$
O\left(
\min\left(np^2,n^2p\right)
\right)
$$

Input storage is:

$$
O(np)
$$

and driver workspace may be substantial.

## Numerical Features

- Returns an effective rank.
- Can return singular values for relevant drivers.
- Handles overdetermined and underdetermined systems.
- Uses rank thresholds governed by `cond`.

## Use

Use `scipy.linalg.lstsq` when direct control over dense least-squares drivers and diagnostic outputs is useful.
