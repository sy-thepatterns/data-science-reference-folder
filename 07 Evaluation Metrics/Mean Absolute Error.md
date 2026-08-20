---
type: metric
name: Mean Absolute Error
status: stub
tags: []
---

# Mean Absolute Error

## Summary

Average absolute prediction error.

## Formal Definition

$$
\operatorname{MAE}
=
\frac{1}{n}
\sum_{i=1}^{n}
\left|y_i-\hat y_i\right|
$$

## Notation

| Symbol | Meaning |
|---|---|
| $n$ | Number of evaluated examples. |
| $i$ | Index of one evaluated example. |
| $y_i$ | Observed target for example $i$. |
| $\hat y_i$ | Prediction for example $i$. |
| $|y_i-\hat y_i|$ | Absolute error: the nonnegative distance between observation and prediction. |
| $\operatorname{MAE}$ | Arithmetic mean of the absolute errors. |

## Intuition

For every prediction, measure how far it missed without caring whether it was too high or too low, then average those distances. An MAE of 3 means predictions miss by 3 target units on average.

## Derivation or Proof

- Show that absolute loss is minimized by a median, which explains why it targets a different centre than squared loss.
- Prove nonnegativity directly from the absolute-value definition.
- Use the triangle inequality to study how MAE changes when predictions are perturbed.

## Complexity

Computing MAE from existing predictions takes $O(n)$ time and $O(1)$ additional streaming storage.

## Related Notes

## References

1. [scikit-learn — mean absolute error](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html). Official documentation — definition, weighting, multioutput behavior, and API.
2. [Hyndman and Koehler — “Another Look at Measures of Forecast Accuracy”](https://robjhyndman.com/papers/mase.pdf). Open paper — absolute and squared forecast errors, scale, and metric comparison.
3. [Murphy — *Probabilistic Machine Learning: An Introduction*](https://probml.github.io/pml-book/book1.html). Open textbook — probability, statistics, supervised learning, optimization, linear algebra, and probabilistic modelling.
4. [James et al. — *An Introduction to Statistical Learning*](https://www.statlearning.com/). Open textbook — accessible statistical explanations, resampling, regression, classification, trees, support vector machines, and unsupervised learning.
