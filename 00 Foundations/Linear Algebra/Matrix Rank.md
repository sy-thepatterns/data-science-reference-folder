---
type: mathematical-foundation
name: Matrix Rank
domain:
  - Linear Algebra
used_by:
  - "[[Linear Regression]]"
  - "[[Singular Value Decomposition]]"
status: developing
tags:
  - linear-algebra
---

# Matrix Rank

## Definition

The rank of a matrix is the dimension of its column space, equivalently its row space.

For:

$$
X \in \mathbb{R}^{n \times p}
$$

full column rank means:

$$
\operatorname{rank}(X)=p
$$

## Relevance to Linear Regression

When $$X$$ has full column rank, $$X^{T}X$$ is invertible and the ordinary least-squares coefficient vector is unique.

When rank is deficient, multiple coefficient vectors may produce the same fitted values. A pseudoinverse or rank-revealing least-squares solver can return a minimum-norm solution.
