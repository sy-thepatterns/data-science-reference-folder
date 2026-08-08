---
type: metric
name: R Squared
aliases:
  - R²
  - Coefficient of Determination
tasks:
  - "[[Regression]]"
range: Usually at most 1; can be negative out of sample
ideal_value: 1
status: reviewed
tags:
  - regression
---

# R Squared

## Formal Definition

$$
R^2
=
1-
\frac{
\sum_{i=1}^{n}
\left(y_i-\hat{y}_i\right)^2
}{
\sum_{i=1}^{n}
\left(y_i-\bar{y}\right)^2
}
$$

where:

$$
\bar{y}
=
\frac{1}{n}
\sum_{i=1}^{n}y_i
$$

## Interpretation

$$R^2$$ compares the model's squared error with the squared error of predicting the sample mean.

## Range

On training data with an intercept and ordinary least squares, $$R^2$$ is typically between zero and one. On held-out data, it can be negative when the model performs worse than the mean baseline.

## Complexity

Computing predictions for linear regression costs:

$$
O(np)
$$

and aggregating the numerator and denominator costs:

$$
O(n)
$$
