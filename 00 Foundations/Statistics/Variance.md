---
type: mathematical-foundation
name: Variance
domain:
  - Probability and Statistics
prerequisites: []
used_by:
  - "[[Linear Regression]]"
  - "[[Expected Value]]"
  - "[[Bias-Variance Decomposition]]"
related: []
status: complete
tags:
  - probability-statistics
---

# Variance

## Definition

For square-integrable $Z$, $\operatorname{Var}(Z)=\mathbb{E}[(Z-\mathbb{E}[Z])^2]=\mathbb{E}[Z^2]-\mathbb{E}[Z]^2$.

## Formal Statement

Variance is nonnegative and $\operatorname{Var}(aZ+b)=a^2\operatorname{Var}(Z)$. For dependent sums, covariance terms must be retained.

## Notation

| Symbol | Meaning |
|---|---|
| $Z$ | Random variable whose spread is measured. |
| $\mathbb{E}[Z]$ | Mean or balance point of $Z$. |
| $\operatorname{Var}(Z)$ | Expected squared distance from $Z$ to its mean. |
| $a,b$ | Fixed constants in a rescaling and shift. |
| $X,y$ | Input matrix and response vector in the regression example. |
| $\varepsilon$ | Regression error vector. |
| $\sigma^2$ | Common error variance under homoscedasticity. |
| $I$ | Identity matrix, meaning different error coordinates have zero covariance in the stated model. |
| $\operatorname{Cov}$ | Covariance, which measures how two random quantities vary together. |

## Intuition

Two classes can have the same average test score but feel very different: one class might cluster near the average, while another has many very high and very low scores. Variance measures that spread by squaring each distance from the average, so opposite directions cannot cancel.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Expand the square to prove $\operatorname{Var}(Z)=\mathbb{E}[Z^2]-\mathbb{E}[Z]^2$.
- Substitute $aZ+b$ into the definition to prove $\operatorname{Var}(aZ+b)=a^2\operatorname{Var}(Z)$.
- Expand the variance of a sum to derive covariance terms and see exactly when variances may be added.

## Geometric or Statistical Interpretation

Variance measures squared dispersion around the mean, not uncertainty about a parameter unless a probabilistic model gives that interpretation.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

One-pass stable estimators such as Welford's algorithm cost $O(n)$ time and constant storage; naive subtraction can suffer catastrophic cancellation.

## Numerical Considerations

Finite-precision results depend on conditioning, scaling, data representation, precision, and the chosen stable algorithm. Mathematical equality must not be confused with floating-point equality.

## Used By

- [[Linear Regression]]
- [[Expected Value]]
- [[Bias-Variance Decomposition]]

## Depends On

- Basic arithmetic and the definitions stated above.

## Related Concepts

- [[Linear Algebra]]
- [[Probability]]
- [[Numerical Stability]]

## References

- Strang, *Introduction to Linear Algebra*, 6th ed., 2023.
- Wasserman, *All of Statistics*, 2004.
