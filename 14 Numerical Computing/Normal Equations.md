---
type: numerical-method
name: Normal Equations
foundations:
  - "[[Least Squares]]"
  - "[[Matrix Multiplication]]"
solves:
  - "[[Least Squares]]"
used_by:
  - "[[Linear Regression]]"
hardware:
  - "[[CPU]]"
status: reviewed
tags:
  - dense
  - linear-algebra
---

# Normal Equations

## Method

For ordinary least squares, solve:

$$
X^{T}X\hat{\beta}=X^{T}y
$$

If $X^{T}X$ is invertible:

$$
\hat{\beta}
=
\left(X^{T}X\right)^{-1}X^{T}y
$$

In numerical code, solve the linear system rather than explicitly computing the inverse.

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

## Complexity

For:

$$
X \in \mathbb{R}^{n \times p}
$$

1. Form $X^{T}X$:

$$
O(np^2)
$$

2. Form $X^{T}y$:

$$
O(np)
$$

3. Solve the $p\times p$ system, commonly by Cholesky or LU-type factorization:

$$
O(p^3)
$$

Overall:

$$
O(np^2+p^3)
$$

Additional dense memory is approximately:

$$
O(p^2)
$$

beyond storage of the input.

## Numerical Stability

The condition number approximately satisfies:

$$
\kappa(X^{T}X)=\kappa(X)^2
$$

Therefore forming $X^{T}X$ may amplify conditioning problems.

## Use

Useful for derivation and sometimes efficient when $p$ is small and the matrix is well-conditioned. It is not the safest default for rank-deficient or ill-conditioned data.
