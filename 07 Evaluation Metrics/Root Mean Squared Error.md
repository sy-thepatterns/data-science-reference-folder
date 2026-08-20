---
type: metric
name: Root Mean Squared Error
status: stub
tags: []
---

# Root Mean Squared Error

## Summary

Square root of mean squared error, expressed in the target's units.

## Formal Definition

$$
\operatorname{RMSE}
=
\sqrt{
\frac{1}{n}
\sum_{i=1}^{n}
\left(y_i-\hat y_i\right)^2
}
$$

## Notation

| Symbol | Meaning |
|---|---|
| $n$ | Number of evaluated examples. |
| $i$ | Index of one evaluated example. |
| $y_i$ | Observed target for example $i$. |
| $\hat y_i$ | Prediction for example $i$. |
| $(y_i-\hat y_i)^2$ | Squared error for example $i$. |
| $\operatorname{RMSE}$ | Square root of the mean squared error, expressed in the target's units. |

## Intuition

RMSE is like measuring a typical miss, but large misses receive extra attention because errors are squared before averaging. Taking the square root at the end returns the answer to the original units, such as dollars or degrees.

## Derivation or Proof

- Substitute the definition of [[Mean Squared Error]] to prove $\operatorname{RMSE}=\sqrt{\operatorname{MSE}}$.
- Prove RMSE is nonnegative because it is the principal square root of an average of squares.
- Compare RMSE with MAE using the root-mean-square–arithmetic-mean inequality applied to absolute errors.

## Complexity

Computing RMSE from existing predictions takes $O(n)$ time and $O(1)$ additional streaming storage.

## Related Notes

## References

1. [scikit-learn — root mean squared error](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.root_mean_squared_error.html). Official documentation — definition, weighting, multioutput behavior, and API.
2. [Hyndman and Koehler — forecast accuracy measures](https://robjhyndman.com/papers/mase.pdf). Open paper — RMSE, MAE, scaling, and comparative properties.
3. [Murphy — *Probabilistic Machine Learning: An Introduction*](https://probml.github.io/pml-book/book1.html). Open textbook — probability, statistics, supervised learning, optimization, linear algebra, and probabilistic modelling.
4. [James et al. — *An Introduction to Statistical Learning*](https://www.statlearning.com/). Open textbook — accessible statistical explanations, resampling, regression, classification, trees, support vector machines, and unsupervised learning.
