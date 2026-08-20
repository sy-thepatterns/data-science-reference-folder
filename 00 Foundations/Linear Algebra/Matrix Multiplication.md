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

For $A\in\mathbb{R}^{m\times k}$ and $B\in\mathbb{R}^{k\times n}$, $C=AB$ has $C_{ij}=\sum_{r=1}^{k}A_{ir}B_{rj}$.

## Formal Statement

The product represents composition of linear maps and is associative but generally not commutative. Transposition reverses order: $(AB)^T=B^TA^T$.

## Notation

| Symbol | Meaning |
|---|---|
| $A,B$ | Input matrices whose inner dimensions agree. |
| $m,k,n$ | Row, shared-inner, and column dimensions in $A\in\mathbb{R}^{m\times k}$ and $B\in\mathbb{R}^{k\times n}$. |
| $C=AB$ | Product matrix in $\mathbb{R}^{m\times n}$. |
| $A_{ir}$ | Entry in row $i$ and column $r$ of $A$. |
| $B_{rj}$ | Entry in row $r$ and column $j$ of $B$. |
| $T(m,k,n)$ | Runtime expressed as a function of the three dimensions. |
| $O(\cdot)$ | Asymptotic growth rate, not an exact operation count. |

## Intuition

A matrix can be read as a machine that transforms an input vector. Multiplying two matrices means connecting two machines: the right-hand one acts first, then the left-hand one. The shared dimension is the size of the message passed between them.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Derive the entry formula by applying the composed linear maps to a basis vector.
- Prove associativity $(AB)C=A(BC)$ by expanding both sides as the same finite double sum.
- Prove $(AB)^T=B^TA^T$ by comparing entry $(i,j)$ on both sides.

## Geometric or Statistical Interpretation

Each output column is a linear combination of columns of $A$; each output row is a combination of rows of $B$.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

Classical dense multiplication costs $O(mkn)$ time and $O(mn)$ output storage; actual performance depends on sparsity, layout, blocking, precision, and backend.

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

1. [Hefferon — *Linear Algebra*](https://hefferon.net/linearalgebra/). Open textbook — linear-map composition and matrix products.
2. [BLAS — Level 3 routines](https://www.netlib.org/blas/#_level_3). Open reference — standard matrix–matrix operations and low-level interfaces.
3. [Murphy — *Probabilistic Machine Learning: An Introduction*](https://probml.github.io/pml-book/book1.html). Open textbook — probability, statistics, supervised learning, optimization, linear algebra, and probabilistic modelling.
4. [MIT OpenCourseWare — Linear Algebra](https://ocw.mit.edu/courses/18-06-linear-algebra-spring-2010/). Open course — linear algebra definitions, proofs, matrix factorizations, and applications.
