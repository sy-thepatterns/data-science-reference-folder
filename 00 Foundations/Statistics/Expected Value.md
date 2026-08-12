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

Expected value is integration with respect to a probability measure: $\mathbb{E}[Z]=\int Z\,dP$ when $Z$ is integrable.

## Formal Statement

Linearity gives $\mathbb{E}[aX+bY]=a\mathbb{E}[X]+b\mathbb{E}[Y]$ without requiring independence. Conditional expectation satisfies the tower property.

## Notation

| Symbol | Meaning |
|---|---|
| $Z$ | Random variable whose long-run probability-weighted average is being described. |
| $z$ | One possible value of $Z$. |
| $P(Z=z)$ | Probability that a discrete $Z$ equals $z$. |
| $f_Z(z)$ | Probability density of a continuous $Z$ at $z$. |
| $\mathbb{E}[Z]$ | Expected value of $Z$, when the defining sum or integral exists. |
| $X,y$ | Input matrix and response vector in the regression example. |
| $\beta$ | Linear-model coefficient vector. |
| $\varepsilon$ | Random error vector. |
| $\mid$ | Conditioning bar: ‘given’ the information on its right. |
| $a,b$ | Fixed scalar multipliers in the linearity identity. |

## Intuition

Expected value is the balance point of a probability distribution. Imagine placing weights at every possible outcome, with heavier weights where outcomes are more likely. The point where the ruler balances is the expectation—even if that exact value can never occur, like an average of 3.5 people.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Prove linearity of expectation by distributing sums or integrals; independence is not needed.
- Derive the tower property $\mathbb{E}[\mathbb{E}[Z\mid X]]=\mathbb{E}[Z]$ by partitioning outcomes according to $X$.
- Show that the expectation minimizes mean squared distance among constant predictions by expanding $\mathbb{E}[(Z-c)^2]$ around $c=\mathbb{E}[Z]$.

## Geometric or Statistical Interpretation

It is a probability-weighted center and the optimal constant predictor under squared loss when the second moment exists.

## Algorithmic Form

There are multiple valid computational procedures. The mathematical object does not specify a numerical algorithm, software implementation, low-level backend, or hardware device.

## Computational Complexity

Exact cost depends on the distribution; Monte Carlo estimation from $n$ samples costs $O(n)$ and has sampling error typically proportional to $n^{-1/2}$ under finite variance.

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
