---
type: mathematical-foundation
name: Vector Space
domain:
  - Linear Algebra
prerequisites: []
used_by:
  - "[[Linear Regression]]"
  - "[[Principal Component Analysis]]"
  - "[[Gradient Descent]]"
related: []
status: complete
tags:
  - linear-algebra
---

# Vector Space

## Definition

A vector space over a field $\mathbb{F}$ is a set with addition and scalar multiplication satisfying closure, associativity, commutativity of addition, identities, inverses, and distributive and compatibility axioms.

## Formal Statement

A subset $U\subseteq V$ is a subspace when it contains zero and is closed under linear combinations. A basis is a linearly independent spanning set; its size is the dimension.

## Notation

| Symbol | Meaning |
|---|---|
| $V$ | Set of objects called vectors. |
| $\mathbb{F}$ | Field of scalars, commonly the real numbers $\mathbb{R}$. |
| $u,v,w$ | Arbitrary vectors in $V$. |
| $a,b$ | Arbitrary scalars in $\mathbb{F}$. |
| $0$ | Zero vector, the additive identity. |
| $\mathbb{R}^p$ | Space of real coordinate vectors with $p$ entries. |
| $p$ | Dimension or number of coordinates in the example space. |
| $U\subseteq V$ | A subset $U$ of $V$, potentially a subspace. |
| $\operatorname{span}$ | All linear combinations of a collection of vectors. |
| $\dim(V)$ | Number of vectors in any basis of finite-dimensional $V$. |

## Intuition

A vector space is a world where arrows can be added and stretched without leaving that world. The rules guarantee that rearranging those ordinary operations does not produce surprises. Coordinates are only one naming system for the arrows; the abstract vector exists independently of the chosen axes.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Verify the vector-space axioms for $\mathbb{R}^p$ coordinate by coordinate.
- Use the subspace test to prove a span is a subspace: it contains zero and is closed under linear combinations.
- Prove that every basis of a finite-dimensional vector space has the same size using the exchange lemma.

## Geometric or Statistical Interpretation

Feature, parameter, gradient, and representation vectors live in spaces such as $\mathbb{R}^p$. Columns of a design matrix span a subspace of possible fitted vectors.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

Coordinate addition and scalar multiplication cost $O(p)$ for dense vectors; sparse representations cost with the number of stored entries.

## Numerical Considerations

Finite-precision results depend on conditioning, scaling, data representation, precision, and the chosen stable algorithm. Mathematical equality must not be confused with floating-point equality.

## Used By

- [[Linear Regression]]
- [[Principal Component Analysis]]
- [[Gradient Descent]]

## Depends On

- Basic arithmetic and the definitions stated above.

## Related Concepts

- [[Linear Algebra]]
- [[Probability]]
- [[Numerical Stability]]

## References

- Strang, *Introduction to Linear Algebra*, 6th ed., 2023.
- Wasserman, *All of Statistics*, 2004.
