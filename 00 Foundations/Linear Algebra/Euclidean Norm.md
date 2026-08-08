---
type: mathematical-foundation
name: Euclidean Norm
domain:
  - Linear Algebra
used_by:
  - "[[Least Squares]]"
  - "[[Mean Squared Error]]"
status: reviewed
tags:
  - linear-algebra
---

# Euclidean Norm

## Definition

For:

$$
v=(v_1,\dots,v_n)^{T}
$$

the Euclidean norm is:

$$
\lVert v\rVert_2
=
\sqrt{
\sum_{i=1}^{n}v_i^2
}
$$

Its square is:

$$
\lVert v\rVert_2^2
=
v^{T}v
$$

## Relevance

Ordinary least squares minimizes the squared Euclidean norm of the residual vector:

$$
\lVert y-X\beta\rVert_2^2
$$
