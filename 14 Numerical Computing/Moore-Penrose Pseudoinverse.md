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

For a matrix $X$, the Moore-Penrose pseudoinverse $X^{+}$ is the unique matrix satisfying four Penrose conditions:

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

## Notation

| Symbol | Meaning |
|---|---|
| $X$ | Possibly rectangular or rank-deficient matrix. |
| $X^+$ | Unique Moore–Penrose pseudoinverse. |
| $U,\Sigma,V$ | Left singular vectors, singular values, and right singular vectors in the SVD. |
| $\Sigma^+$ | Matrix made by reciprocating retained nonzero singular values and transposing the rectangular shape. |
| $y$ | Target or right-hand-side vector. |
| $\hat\beta$ | Minimum-Euclidean-norm solution among all least-squares minimizers. |
| $T$ | Transpose operation when written as a superscript. |

## Intuition

An ordinary inverse runs a reversible machine backward. A rectangular or rank-deficient matrix is not fully reversible, so the pseudoinverse does the fairest possible reverse: it matches what can be matched and chooses the shortest input among equally good answers.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Insert $X=U\Sigma V^T$ and $X^+=V\Sigma^+U^T$ into each of the four Penrose conditions.
- Use orthogonal decomposition to prove $X^+y$ minimizes residual length.
- Show that adding any null-space vector preserves the fit but increases or leaves unchanged the coefficient norm, proving the minimum-norm property.

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
