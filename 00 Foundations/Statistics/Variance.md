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

For square-integrable $$Z$$, $$\operatorname{Var}(Z)=\mathbb{E}[(Z-\mathbb{E}[Z])^2]=\mathbb{E}[Z^2]-\mathbb{E}[Z]^2$$.

## Formal Statement

Variance is nonnegative and $$\operatorname{Var}(aZ+b)=a^2\operatorname{Var}(Z)$$. For dependent sums, covariance terms must be retained.

## Notation

Symbols denote mathematical objects independently of any array class, numerical routine, library, or processor. Dimensions and assumptions should be declared where the concept is used.

## Intuition

Variance measures squared dispersion around the mean, not uncertainty about a parameter unless a probabilistic model gives that interpretation.

## Derivation or Proof

The defining identities follow from the underlying algebra or probability axioms. A full proof depends on the selected field and is separate from numerical procedures used to approximate the quantity.

## Geometric or Statistical Interpretation

Variance measures squared dispersion around the mean, not uncertainty about a parameter unless a probabilistic model gives that interpretation.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

One-pass stable estimators such as Welford's algorithm cost $$O(n)$$ time and constant storage; naive subtraction can suffer catastrophic cancellation.

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
