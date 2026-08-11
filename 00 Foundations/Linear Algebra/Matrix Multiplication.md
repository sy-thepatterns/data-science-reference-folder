---
type: mathematical-foundation
name: Matrix Multiplication
domain:
  - Linear Algebra
prerequisites: []
used_by:
  - "[[Linear Regression]]"
  - "[[Normal Equations]]"
  - "[[Neural Networks]]"
related: []
status: complete
tags:
  - linear-algebra
---

# Matrix Multiplication

## Definition

For $$A\in\mathbb{R}^{m\times k}$$ and $$B\in\mathbb{R}^{k\times n}$$, $$C=AB$$ has $$C_{ij}=\sum_{r=1}^{k}A_{ir}B_{rj}$$.

## Formal Statement

The product represents composition of linear maps and is associative but generally not commutative. Transposition reverses order: $$(AB)^T=B^TA^T$$.

## Notation

Symbols denote mathematical objects independently of any array class, numerical routine, library, or processor. Dimensions and assumptions should be declared where the concept is used.

## Intuition

Each output column is a linear combination of columns of $$A$$; each output row is a combination of rows of $$B$$.

## Derivation or Proof

The defining identities follow from the underlying algebra or probability axioms. A full proof depends on the selected field and is separate from numerical procedures used to approximate the quantity.

## Geometric or Statistical Interpretation

Each output column is a linear combination of columns of $$A$$; each output row is a combination of rows of $$B$$.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

Classical dense multiplication costs $$O(mkn)$$ time and $$O(mn)$$ output storage; actual performance depends on sparsity, layout, blocking, precision, and backend.

## Numerical Considerations

Finite-precision results depend on conditioning, scaling, data representation, precision, and the chosen stable algorithm. Mathematical equality must not be confused with floating-point equality.

## Used By

- [[Linear Regression]]
- [[Normal Equations]]
- [[Neural Networks]]

## Depends On

- Basic arithmetic and the definitions stated above.

## Related Concepts

- [[Linear Algebra]]
- [[Probability]]
- [[Numerical Stability]]

## References

- Strang, *Introduction to Linear Algebra*, 6th ed., 2023.
- Wasserman, *All of Statistics*, 2004.
