---
type: mathematical-foundation
name: Expected Value
domain:
  - Probability and Statistics
prerequisites: []
used_by:
  - "[[Linear Regression]]"
  - "[[Variance]]"
  - "[[Bayesian Methods]]"
related: []
status: complete
tags:
  - probability-statistics
---

# Expected Value

## Definition

Expected value is integration with respect to a probability measure: $$\mathbb{E}[Z]=\int Z\,dP$$ when $$Z$$ is integrable.

## Formal Statement

Linearity gives $$\mathbb{E}[aX+bY]=a\mathbb{E}[X]+b\mathbb{E}[Y]$$ without requiring independence. Conditional expectation satisfies the tower property.

## Notation

Symbols denote mathematical objects independently of any array class, numerical routine, library, or processor. Dimensions and assumptions should be declared where the concept is used.

## Intuition

It is a probability-weighted center and the optimal constant predictor under squared loss when the second moment exists.

## Derivation or Proof

The defining identities follow from the underlying algebra or probability axioms. A full proof depends on the selected field and is separate from numerical procedures used to approximate the quantity.

## Geometric or Statistical Interpretation

It is a probability-weighted center and the optimal constant predictor under squared loss when the second moment exists.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

Exact cost depends on the distribution; Monte Carlo estimation from $$n$$ samples costs $$O(n)$$ and has sampling error typically proportional to $$n^{-1/2}$$ under finite variance.

## Numerical Considerations

Finite-precision results depend on conditioning, scaling, data representation, precision, and the chosen stable algorithm. Mathematical equality must not be confused with floating-point equality.

## Used By

- [[Linear Regression]]
- [[Variance]]
- [[Bayesian Methods]]

## Depends On

- Basic arithmetic and the definitions stated above.

## Related Concepts

- [[Linear Algebra]]
- [[Probability]]
- [[Numerical Stability]]

## References

- Strang, *Introduction to Linear Algebra*, 6th ed., 2023.
- Wasserman, *All of Statistics*, 2004.
