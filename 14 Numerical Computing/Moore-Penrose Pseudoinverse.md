---
type: numerical-method
name: Moore-Penrose Pseudoinverse
aliases:
  - Pseudoinverse
foundations:
  - "[[Singular Value Decomposition]]"
used_by:
  - "[[Linear Regression]]"
status: reviewed
tags:
  - linear-algebra
---

# Moore-Penrose Pseudoinverse

## Definition

For a matrix $$X$$, the Moore-Penrose pseudoinverse $$X^{+}$$ is the unique matrix satisfying four Penrose conditions:

$$
XX^{+}X=X
$$

$$
X^{+}XX^{+}=X^{+}
$$

$$
\left(XX^{+}\right)^{T}=XX^{+}
$$

$$
\left(X^{+}X\right)^{T}=X^{+}X
$$

## Construction by SVD

If:

$$
X=U\Sigma V^{T}
$$

then:

$$
X^{+}=V\Sigma^{+}U^{T}
$$

where nonzero singular values are reciprocated and values treated as numerically zero are not.

## Least Squares

$$
\hat{\beta}=X^{+}y
$$

is a least-squares solution. When multiple solutions exist, it is the minimum-norm solution.
