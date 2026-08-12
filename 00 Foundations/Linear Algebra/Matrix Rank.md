---
type: mathematical-foundation
name: Matrix Rank
domain:
  - Linear Algebra
prerequisites: []
used_by:
  - "[[Linear Regression]]"
  - "[[Singular Value Decomposition]]"
  - "[[Least Squares]]"
related: []
status: complete
tags:
  - linear-algebra
---

# Matrix Rank

## Definition

Matrix rank is the dimension of the column space, equivalently the row space and the number of nonzero singular values.

## Formal Statement

For $A\in\mathbb{R}^{m\times n}$, $0\le\operatorname{rank}(A)\le\min(m,n)$ and $\operatorname{rank}(A)=n$ means full column rank.

## Notation

| Symbol | Meaning |
|---|---|
| $A,X$ | A matrix viewed as a linear map or collection of row and column vectors. |
| $\operatorname{rank}(A)$ | Number of linearly independent columns, equivalently independent rows or nonzero singular values. |
| $m,n,p$ | Matrix dimensions; context states which symbol counts rows or columns. |
| $\ker(A)$ | Null space: vectors mapped to zero by $A$. |
| $\dim$ | Dimension: number of vectors in a basis. |
| $X^TX$ | Gram matrix of the columns of $X$. |

## Intuition

Rank counts how many genuinely different directions a matrix contains. If one column can be rebuilt from the others, it adds no new direction and does not increase rank. Low rank therefore means some recorded columns are repeats or mixtures of the same underlying information.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Prove row rank equals column rank using Gaussian elimination and the invariance of rank under elementary row operations.
- Use the rank-nullity theorem to show $\operatorname{rank}(A)+\dim\ker(A)=n$ for a matrix with $n$ columns.
- Use the SVD to prove that rank equals the number of nonzero singular values.

## Geometric or Statistical Interpretation

Rank counts independent directions preserved by a linear map. The nullity satisfies $\operatorname{rank}(A)+\dim\ker(A)=n$.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

Exact symbolic elimination is polynomial; numerical rank is determined with a tolerance using pivoted QR or singular values and depends on scale and precision.

## Numerical Considerations

Finite-precision results depend on conditioning, scaling, data representation, precision, and the chosen stable algorithm. Mathematical equality must not be confused with floating-point equality.

## Used By

- [[Linear Regression]]
- [[Singular Value Decomposition]]
- [[Least Squares]]

## Depends On

- Basic arithmetic and the definitions stated above.

## Related Concepts

- [[Linear Algebra]]
- [[Probability]]
- [[Numerical Stability]]

## References

- Strang, *Introduction to Linear Algebra*, 6th ed., 2023.
- Wasserman, *All of Statistics*, 2004.
