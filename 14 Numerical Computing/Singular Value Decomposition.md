---
type: numerical-method
name: Singular Value Decomposition
aliases:
  - SVD
foundations:
  - "[[Matrix Rank]]"
solves:
  - "[[Least Squares]]"
used_by:
  - "[[Linear Regression]]"
  - "[[Moore-Penrose Pseudoinverse]]"
libraries:
  - "[[LAPACK]]"
hardware:
  - "[[CPU]]"
status: reviewed
tags:
  - linear-algebra
  - rank-revealing
  - numerically-stable
---

# Singular Value Decomposition

## Definition

For:

$$
X \in \mathbb{R}^{n \times p}
$$

the singular value decomposition is:

$$
X=U\Sigma V^{T}
$$

where the columns of $U$ and $V$ are orthonormal and $\Sigma$ contains nonnegative singular values.

## Notation

| Symbol | Meaning |
|---|---|
| $X$ | Input matrix in $\mathbb{R}^{n\times p}$. |
| $U$ | Matrix of orthonormal left singular vectors. |
| $V$ | Matrix of orthonormal right singular vectors. |
| $\Sigma$ | Diagonal or rectangular-diagonal matrix of nonnegative singular values. |
| $X^+$ | Moore–Penrose pseudoinverse of $X$. |
| $\Sigma^+$ | Pseudoinverse formed by reciprocating retained nonzero singular values. |
| $y$ | Right-hand-side or target vector. |
| $\hat\beta$ | Minimum-norm least-squares coefficient solution under the standard cutoff. |
| $n,p$ | Matrix row and column counts. |
| $O(\cdot)$ | Asymptotic growth rate. |

## Intuition

The SVD says any matrix transformation can be understood as three moves: rotate the input, stretch or squash each perpendicular direction, then rotate again. A zero stretch destroys a direction; a tiny stretch makes reversing the transformation dangerously sensitive to noise.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Construct the right singular vectors from the eigenvectors of $X^TX$, then obtain left singular vectors from $Xv_i/\sigma_i$.
- Verify the four Penrose conditions for $X^+=V\Sigma^+U^T$.
- Prove the Eckart–Young–Mirsky theorem: truncating to the largest singular values gives the best low-rank approximation; see [[Low-Rank Approximation]].

## Least-Squares Solution

Using the pseudoinverse:

$$
X^{+}
=
V\Sigma^{+}U^{T}
$$

a least-squares solution is:

$$
\hat{\beta}=X^{+}y
$$

For rank-deficient systems, this gives the minimum-Euclidean-norm solution under the standard pseudoinverse convention.

## Complexity

A dense SVD has shape-dependent complexity. A useful summary is:

$$
O\left(
\min\left(np^2,n^2p\right)
\right)
$$

for the dominant factorization work.

## Strengths

- Reveals numerical rank.
- Handles rank deficiency.
- Provides singular values and conditioning information.
- Avoids the instability of explicitly forming $X^{T}X$.

## Limitations

- Usually more computationally expensive than a basic QR solve.
- Can require substantial workspace.
