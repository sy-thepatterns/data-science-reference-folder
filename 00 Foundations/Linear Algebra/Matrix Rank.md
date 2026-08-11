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

For $$A\in\mathbb{R}^{m\times n}$$, $$0\le\operatorname{rank}(A)\le\min(m,n)$$ and $$\operatorname{rank}(A)=n$$ means full column rank.

## Notation

Symbols denote mathematical objects independently of any array class, numerical routine, library, or processor. Dimensions and assumptions should be declared where the concept is used.

## Intuition

Rank counts independent directions preserved by a linear map. The nullity satisfies $$\operatorname{rank}(A)+\dim\ker(A)=n$$.

## Derivation or Proof

The defining identities follow from the underlying algebra or probability axioms. A full proof depends on the selected field and is separate from numerical procedures used to approximate the quantity.

## Geometric or Statistical Interpretation

Rank counts independent directions preserved by a linear map. The nullity satisfies $$\operatorname{rank}(A)+\dim\ker(A)=n$$.

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
