---
type: mathematical-foundation
name: Variance
domain:
  - Statistics
used_by:
  - "[[Linear Regression]]"
status: developing
tags:
  - statistics
---

# Variance

## Definition

$$
\operatorname{Var}(Z)
=
\mathbb{E}
\left[
\left(
Z-\mathbb{E}[Z]
\right)^2
\right]
$$

## Linear Regression Relevance

Under homoscedastic errors:

$$
\operatorname{Var}(\varepsilon \mid X)
=
\sigma^2 I
$$

This assumption is important for the classical ordinary-least-squares variance formula and standard inference.
