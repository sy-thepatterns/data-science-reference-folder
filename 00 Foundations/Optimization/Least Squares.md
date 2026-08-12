---
type: mathematical-foundation
name: Least Squares
domain:
  - Optimization
prerequisites: []
used_by:
  - "[[Linear Regression]]"
  - "[[Ridge Regression]]"
  - "[[Matrix Factorization]]"
related: []
status: complete
tags:
  - optimization
---

# Least Squares

## Definition

Given $A\in\mathbb{R}^{m\times n}$ and $b\in\mathbb{R}^m$, solve $x^{\star}=\arg\min_x\lVert Ax-b\rVert_2^2$.

## Formal Statement

The gradient is $2A^T(Ax-b)$ and the Hessian $2A^TA\succeq0$. At an optimum, $A^T(Ax-b)=0$.

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

## Geometric or Statistical Interpretation

The fitted vector is an orthogonal projection onto the column space of $A$; a rank-deficient problem may have many coefficient solutions but one fitted vector.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

Dense solver costs differ: normal equations, QR, and SVD have different constants and stability; iterative solvers depend on sparsity, conditioning, tolerance, and iterations.

## Numerical Considerations

Finite-precision results depend on conditioning, scaling, data representation, precision, and the chosen stable algorithm. Mathematical equality must not be confused with floating-point equality.

## Used By

- [[Linear Regression]]
- [[Ridge Regression]]
- [[Matrix Factorization]]

## Depends On

- Basic arithmetic and the definitions stated above.

## Related Concepts

- [[Linear Algebra]]
- [[Probability]]
- [[Numerical Stability]]

## References

- Strang, *Introduction to Linear Algebra*, 6th ed., 2023.
- Wasserman, *All of Statistics*, 2004.
