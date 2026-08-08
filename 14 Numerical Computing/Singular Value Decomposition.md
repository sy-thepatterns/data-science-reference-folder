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

where the columns of $$U$$ and $$V$$ are orthonormal and $$\Sigma$$ contains nonnegative singular values.

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
- Avoids the instability of explicitly forming $$X^{T}X$$.

## Limitations

- Usually more computationally expensive than a basic QR solve.
- Can require substantial workspace.
