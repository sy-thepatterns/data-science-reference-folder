---
type: mathematical-foundation
name: Least Squares
domain:
  - Optimization
  - Statistics
used_by:
  - "[[Linear Regression]]"
solvers:
  - "[[Normal Equations]]"
  - "[[QR Decomposition]]"
  - "[[Singular Value Decomposition]]"
status: reviewed
tags:
  - convex
  - optimization
---

# Least Squares

## Problem

Given:

$$
A \in \mathbb{R}^{m \times n}
$$

and:

$$
b \in \mathbb{R}^{m}
$$

find:

$$
x^{\star}
=
\arg\min_x
\lVert Ax-b\rVert_2^2
$$

## Geometry

The fitted vector $$Ax^{\star}$$ is the orthogonal projection of $$b$$ onto the column space of $$A$$.

At an optimum, the residual:

$$
r=b-Ax^{\star}
$$

is orthogonal to every column of $$A$$:

$$
A^{T}r=0
$$

Therefore:

$$
A^{T}Ax^{\star}=A^{T}b
$$

These are the normal equations.

## Convexity

The Hessian is:

$$
\nabla^2
\lVert Ax-b\rVert_2^2
=
2A^{T}A
$$

Since $$A^{T}A$$ is positive semidefinite, the problem is convex. It is strictly convex and has a unique minimizer when $$A$$ has full column rank.

## Solver Choice

The normal equations are mathematically direct but can be less numerically stable because forming $$A^{T}A$$ squares the condition number. QR or SVD-based solvers are generally preferred when numerical stability matters.
