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

A vector space over a field $$\mathbb{F}$$ is a set with addition and scalar multiplication satisfying closure, associativity, commutativity of addition, identities, inverses, and distributive and compatibility axioms.

## Formal Statement

A subset $$U\subseteq V$$ is a subspace when it contains zero and is closed under linear combinations. A basis is a linearly independent spanning set; its size is the dimension.

## Notation

Symbols denote mathematical objects independently of any array class, numerical routine, library, or processor. Dimensions and assumptions should be declared where the concept is used.

## Intuition

Feature, parameter, gradient, and representation vectors live in spaces such as $$\mathbb{R}^p$$. Columns of a design matrix span a subspace of possible fitted vectors.

## Derivation or Proof

The defining identities follow from the underlying algebra or probability axioms. A full proof depends on the selected field and is separate from numerical procedures used to approximate the quantity.

## Geometric or Statistical Interpretation

Feature, parameter, gradient, and representation vectors live in spaces such as $$\mathbb{R}^p$$. Columns of a design matrix span a subspace of possible fitted vectors.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

Coordinate addition and scalar multiplication cost $$O(p)$$ for dense vectors; sparse representations cost with the number of stored entries.

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
