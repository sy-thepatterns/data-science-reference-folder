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

If $$X^{T}X$$ is invertible:

$$
\hat{\beta}
=
\left(X^{T}X\right)^{-1}X^{T}y
$$

In numerical code, solve the linear system rather than explicitly computing the inverse.

## Complexity

For:

$$
X \in \mathbb{R}^{n \times p}
$$

1. Form $$X^{T}X$$:

$$
O(np^2)
$$

2. Form $$X^{T}y$$:

$$
O(np)
$$

3. Solve the $$p\times p$$ system, commonly by Cholesky or LU-type factorization:

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

Therefore forming $$X^{T}X$$ may amplify conditioning problems.

## Use

Useful for derivation and sometimes efficient when $$p$$ is small and the matrix is well-conditioned. It is not the safest default for rank-deficient or ill-conditioned data.
