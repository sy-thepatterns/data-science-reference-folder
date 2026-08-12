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

For $v\in\mathbb{R}^n$, $\lVert v\rVert_2=\sqrt{\sum_i v_i^2}=\sqrt{v^Tv}$.

## Formal Statement

It is nonnegative, definite, homogeneous, and satisfies the triangle inequality. The induced distance is $d(x,y)=\lVert x-y\rVert_2$.

## Notation

| Symbol | Meaning |
|---|---|
| $v$ | Vector whose length is measured. |
| $v_i$ | The $i$th coordinate of $v$. |
| $n$ | Number of coordinates in $v$. |
| $v^T$ | Transpose of $v$, written as a row vector. |
| $\lVert v\rVert_2$ | Euclidean length of $v$. |
| $\lVert v\rVert_2^2$ | Squared Euclidean length, equal to $v^Tv$. |
| $Q$ | An orthogonal matrix satisfying $Q^TQ=I$. |
| $I$ | Identity matrix. |

## Intuition

Think of $v$ as directions walked along perpendicular streets: some east, some north, and perhaps more directions we cannot draw. The Euclidean norm is the straight-line distance from where you started to where you ended. Squaring each move prevents positive and negative directions from cancelling.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Prove $\lVert v\rVert_2^2=v^Tv$ by expanding the dot product coordinate by coordinate.
- Prove orthogonal invariance from $\lVert Qv\rVert_2^2=v^TQ^TQv=v^Tv$.
- Prove the triangle inequality from the Cauchy–Schwarz inequality; see [[Cauchy-Schwarz Inequality]].

## Geometric or Statistical Interpretation

The norm measures straight-line distance from the origin. Rotating or reflecting the coordinate axes changes the coordinates used to describe a vector, but it does not change the vector's physical length. That is why an orthogonal transformation $Q$ preserves the norm:

$$
\lVert Qv\rVert_2=\lVert v\rVert_2
$$

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

A dense norm costs $O(n)$ time and constant auxiliary storage; stable implementations use scaled sums of squares to reduce overflow and underflow.

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
