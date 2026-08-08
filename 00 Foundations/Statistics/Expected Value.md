---
type: mathematical-foundation
name: Expected Value
domain:
  - Probability
  - Statistics
used_by:
  - "[[Linear Regression]]"
status: developing
tags:
  - statistics
---

# Expected Value

## Definition

For a discrete random variable $$Z$$:

$$
\mathbb{E}[Z]
=
\sum_{z} z\,P(Z=z)
$$

For a continuous random variable:

$$
\mathbb{E}[Z]
=
\int_{-\infty}^{\infty}
z f_Z(z)\,dz
$$

## Linear Regression Relevance

A common conditional-mean assumption is:

$$
\mathbb{E}[\varepsilon \mid X]=0
$$

which implies:

$$
\mathbb{E}[y\mid X]=X\beta
$$
