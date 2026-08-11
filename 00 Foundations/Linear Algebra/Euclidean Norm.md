---
type: mathematical-foundation
name: Euclidean Norm
domain:
  - Linear Algebra
prerequisites: []
used_by:
  - "[[Least Squares]]"
  - "[[Mean Squared Error]]"
  - "[[Nearest-Neighbour Methods]]"
related: []
status: complete
tags:
  - linear-algebra
---

# Euclidean Norm

## Definition

For $$v\in\mathbb{R}^n$$, $$\lVert v\rVert_2=\sqrt{\sum_i v_i^2}=\sqrt{v^Tv}$$.

## Formal Statement

It is nonnegative, definite, homogeneous, and satisfies the triangle inequality. The induced distance is $$d(x,y)=\lVert x-y\rVert_2$$.

## Notation

Symbols denote mathematical objects independently of any array class, numerical routine, library, or processor. Dimensions and assumptions should be declared where the concept is used.

## Intuition

It is the ordinary geometric length and is invariant under orthogonal transformations.

## Derivation or Proof

The defining identities follow from the underlying algebra or probability axioms. A full proof depends on the selected field and is separate from numerical procedures used to approximate the quantity.

## Geometric or Statistical Interpretation

It is the ordinary geometric length and is invariant under orthogonal transformations.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

A dense norm costs $$O(n)$$ time and constant auxiliary storage; stable implementations use scaled sums of squares to reduce overflow and underflow.

## Numerical Considerations

Finite-precision results depend on conditioning, scaling, data representation, precision, and the chosen stable algorithm. Mathematical equality must not be confused with floating-point equality.

## Used By

- [[Least Squares]]
- [[Mean Squared Error]]
- [[Nearest-Neighbour Methods]]

## Depends On

- Basic arithmetic and the definitions stated above.

## Related Concepts

- [[Linear Algebra]]
- [[Probability]]
- [[Numerical Stability]]

## References

- Strang, *Introduction to Linear Algebra*, 6th ed., 2023.
- Wasserman, *All of Statistics*, 2004.
