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

Given $$A\in\mathbb{R}^{m\times n}$$ and $$b\in\mathbb{R}^m$$, solve $$x^{\star}=\arg\min_x\lVert Ax-b\rVert_2^2$$.

## Formal Statement

The gradient is $$2A^T(Ax-b)$$ and the Hessian $$2A^TA\succeq0$$. At an optimum, $$A^T(Ax-b)=0$$.

## Notation

Symbols denote mathematical objects independently of any array class, numerical routine, library, or processor. Dimensions and assumptions should be declared where the concept is used.

## Intuition

The fitted vector is an orthogonal projection onto the column space of $$A$$; a rank-deficient problem may have many coefficient solutions but one fitted vector.

## Derivation or Proof

The defining identities follow from the underlying algebra or probability axioms. A full proof depends on the selected field and is separate from numerical procedures used to approximate the quantity.

## Geometric or Statistical Interpretation

The fitted vector is an orthogonal projection onto the column space of $$A$$; a rank-deficient problem may have many coefficient solutions but one fitted vector.

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
