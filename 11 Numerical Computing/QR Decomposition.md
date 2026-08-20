---
type: numerical-method
name: QR Decomposition
foundations:
  - "[[Matrix Multiplication]]"
solves:
  - "[[Least Squares]]"
used_by:
  - "[[Linear Regression]]"
hardware:
  - "[[CPU]]"
status: developing
tags:
  - linear-algebra
  - numerically-stable
---

# QR Decomposition

## Factorization

For a tall matrix:

$$
X \in \mathbb{R}^{n \times p}
$$

with $n\ge p$, a reduced QR factorization writes:

$$
X=QR
$$

where:

$$
Q^{T}Q=I
$$

and $R$ is upper triangular.

The least-squares problem becomes:

$$
\min_\beta
\lVert Q R\beta-y\rVert_2^2
$$

leading to:

$$
R\hat{\beta}=Q^{T}y
$$

for full column rank.

## Notation

| Symbol | Meaning |
|---|---|
| $X$ | Input matrix, usually $n\times p$ with $n\ge p$ for a tall least-squares system. |
| $Q$ | Matrix with orthonormal columns; $Q^TQ=I$. |
| $R$ | Upper-triangular matrix. |
| $I$ | Identity matrix. |
| $y$ | Observed right-hand-side vector. |
| $\beta,\hat\beta$ | Candidate and fitted coefficient vectors. |
| $n,p$ | Numbers of rows and columns. |
| $O(\cdot)$ | Asymptotic work estimate. |

## Intuition

QR separates a matrix into directions and scales. $Q$ gives clean perpendicular directions, while $R$ records how the original columns combine those directions. Once the messy geometry is cleaned up, solving least squares becomes a simpler triangular back-substitution problem.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Construct QR with Householder reflections and show each reflection preserves lengths and angles.
- Prove $\lVert QR\beta-y\rVert_2$ splits into a part minimized by $R\hat\beta=Q^Ty$ and an unavoidable orthogonal remainder.
- Prove uniqueness of the reduced factorization when $X$ has full column rank and the diagonal of $R$ is required to be positive.

## Complexity

Dense Householder QR for $n\ge p$ costs approximately:

$$
O(np^2)
$$

with lower-order $O(p^3)$ terms depending on the exact operation and whether the explicit $Q$ is formed.

## Stability

QR avoids explicitly forming $X^{T}X$ and is generally more stable than the normal-equation route.

## Rank-Revealing Variant

Column-pivoted QR can help detect rank deficiency and improve robustness.

## References

1. [Householder — “Unitary Triangularization”](https://doi.org/10.1145/320941.320947). Foundational paper — reflection-based triangularization.
2. [LAPACK — `DGEQRF`](https://www.netlib.org/lapack/explore-html/d0/da1/group__geqrf_gade26961283814bb4e62183d9133d8bf5.html). Backend documentation — blocked Householder QR, reflector storage, and workspace.
3. [Demmel et al. — communication-optimal QR](https://people.eecs.berkeley.edu/~demmel/CommunicationAvoidingAlgorithms.html). Open author resources — parallel QR, data movement, and communication-avoiding factorizations.
4. [MIT OpenCourseWare — Numerical Methods](https://ocw.mit.edu/courses/18-335j-introduction-to-numerical-methods-spring-2019/). Open course — floating-point arithmetic, conditioning, linear systems, least squares, and matrix factorizations.
