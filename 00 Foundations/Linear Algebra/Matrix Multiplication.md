---
type: mathematical-foundation
name: Matrix Multiplication
domain:
  - Linear Algebra
used_by:
  - "[[Linear Regression]]"
  - "[[Normal Equations]]"
status: reviewed
tags:
  - linear-algebra
  - computational-complexity
---

# Matrix Multiplication

## Definition

For:

$$
A \in \mathbb{R}^{m \times k}
$$

and:

$$
B \in \mathbb{R}^{k \times n}
$$

the product $$C=AB$$ has entries:

$$
C_{ij}
=
\sum_{r=1}^{k}
A_{ir}B_{rj}
$$

## Classical Complexity

The direct algorithm computes $$mn$$ output entries, each requiring $$k$$ multiply-add terms:

$$
T(m,k,n)=O(mkn)
$$

The output itself requires:

$$
O(mn)
$$

space.

## Linear Regression Example

For:

$$
X \in \mathbb{R}^{n \times p}
$$

computing:

$$
X^{T}X
$$

with the classical dense algorithm costs:

$$
O(np^2)
$$
