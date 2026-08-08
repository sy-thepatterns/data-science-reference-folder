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

with $$n\ge p$$, a reduced QR factorization writes:

$$
X=QR
$$

where:

$$
Q^{T}Q=I
$$

and $$R$$ is upper triangular.

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

## Complexity

Dense Householder QR for $$n\ge p$$ costs approximately:

$$
O(np^2)
$$

with lower-order $$O(p^3)$$ terms depending on the exact operation and whether the explicit $$Q$$ is formed.

## Stability

QR avoids explicitly forming $$X^{T}X$$ and is generally more stable than the normal-equation route.

## Rank-Revealing Variant

Column-pivoted QR can help detect rank deficiency and improve robustness.
