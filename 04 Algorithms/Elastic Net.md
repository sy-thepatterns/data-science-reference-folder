---
type: algorithm
name: Elastic Net
learning_paradigm:
  - "[[Supervised Learning]]"
task:
  - "[[Regression]]"
family:
  - "[[Linear Models]]"
foundations:
  - "[[Linear Regression]]"
objective:
  - L1- and L2-regularized least squares
loss:
  - "[[Mean Squared Error]]"
optimization:
  - Convex nonsmooth optimization
solvers:
  - Coordinate Descent
  - Proximal Gradient Method
implementations:
  - "[[scikit-learn - ElasticNet]]"
  - "[[statsmodels - Elastic Net via fit_regularized]]"
  - "[[NumPy - Elastic Net]]"
  - "[[SciPy - Elastic Net]]"
  - "[[PyTorch - Elastic Net]]"
  - "[[TensorFlow - Elastic Net]]"
related:
  - "[[Ridge Regression]]"
  - "[[Lasso Regression]]"
status: reviewed
tags:
  - linear
  - parametric
  - frequentist
  - convex
  - nondifferentiable
  - interpretable
  - regularized
  - sparse
  - high-dimensional
---

# Elastic Net

## Overview

Elastic net is a regularized linear regression method that combines the sparsity-inducing penalty of [[Lasso Regression]] with the stabilizing squared penalty of [[Ridge Regression]]. It is especially useful when predictors are numerous and strongly correlated.

## Formal Definition

One common parameterization is:

$$
\hat{\beta}
=
\arg\min_{\beta}
\left\{
\frac{1}{2n}\lVert y-X\beta\rVert_2^2
+
\lambda
\left(
\alpha\lVert\beta\rVert_1
+
\frac{1-\alpha}{2}\lVert\beta\rVert_2^2
\right)
\right\}
$$

with:

$$
\lambda\ge 0
$$

and:

$$
0\le\alpha\le 1
$$

At:

$$
\alpha=1
$$

the objective is lasso under this scaling. At:

$$
\alpha=0
$$

it is ridge. Software packages use different constants and parameter names, so values are not always directly transferable.

## Convexity and Optimality

The squared-error term, absolute-value penalty, and squared penalty are convex. Their nonnegative weighted sum is convex. When the squared component is positive, it adds strict curvature in coefficient directions and can make the coefficient solution unique.

The optimality condition is:

$$
0
\in
\frac{1}{n}X^T(X\hat{\beta}-y)
+
\lambda\alpha\partial\lVert\hat{\beta}\rVert_1
+
\lambda(1-\alpha)\hat{\beta}
$$

## Grouping Behaviour

The squared penalty discourages large differences among coefficients for highly correlated, similarly scaled predictors. Elastic net can therefore retain groups of related variables more readily than lasso, although this is not the same as an explicit group-lasso penalty.

## Statistical Properties

- The absolute-value component can produce exact zeros.
- Both penalty components introduce coefficient bias.
- The squared component can reduce selection instability under collinearity.
- Predictive variance can fall substantially relative to unregularized regression.
- Support recovery still requires assumptions and is not guaranteed by sparsity.

## Optimization and Solvers

Coordinate descent commonly combines a partial-residual update, soft thresholding for the absolute-value term, and additional shrinkage from the squared term. Warm starts make fitting a regularization path efficient.

The model, mixed penalty, coordinate-descent procedure, library implementation, numerical kernels, and hardware backend remain separate concepts.

## Training Pseudocode

```text
INPUT:
    design matrix X
    target y
    overall penalty lambda
    mixing parameter alpha
    tolerance and iteration limit

1. Validate lambda and alpha.
2. Center and scale using training data only.
3. Initialize coefficients or warm-start from a nearby penalty.
4. Apply the selected convex solver until its stopping rule is met.
5. Recover the unpenalized intercept.
6. Store coefficients and preprocessing metadata.

OUTPUT:
    coefficient vector and intercept
```

## Complexity

A dense coordinate sweep is commonly:

$$
O(np)
$$

and:

$$
T
$$

sweeps give the high-level form:

$$
O(Tnp)
$$

Actual work depends on sparsity, active-set screening, warm starts, conditioning, and tolerance. Dense batch prediction costs:

$$
O(mp)
$$

or less when only active coefficients are stored and used.

## Hyperparameters

| Parameter | Mathematical effect | Typical behaviour |
|---|---|---|
| Overall penalty | Controls total shrinkage | Larger values simplify the model |
| Mixing parameter | Trades absolute and squared penalties | Larger values increase lasso-like sparsity |
| Tolerance | Controls solver termination | Smaller values increase computational work |
| Feature scaling | Sets relative coefficient penalization | Standardization is usually essential |

## Advantages

- Combines sparsity with stability under correlated predictors.
- Convex objective.
- Useful in high-dimensional problems.
- Often provides a practical continuum between ridge and lasso.

## Limitations and Failure Modes

- Requires tuning at least two regularization dimensions.
- Coefficients are biased.
- Selected features may still be unstable.
- Mixed-penalty conventions differ across libraries.
- Scaling and validation leakage can invalidate comparisons.
- Squared residual loss is sensitive to large response outliers.

## Diagnostics

- Use nested or otherwise leakage-safe validation for both penalty parameters.
- Plot coefficient paths and active-set sizes.
- Check optimization convergence.
- Assess selection stability under resampling.
- Compare with both endpoint models.

## Related Algorithms

- [[Lasso Regression]] is the pure absolute-penalty endpoint.
- [[Ridge Regression]] is the pure squared-penalty endpoint.
- [[Linear Regression]] is recovered when the overall penalty is zero.

## Implementations

- [[scikit-learn - ElasticNet]]
- [[statsmodels - Elastic Net via fit_regularized]]
- [[NumPy - Elastic Net]]
- [[SciPy - Elastic Net]]
- [[PyTorch - Elastic Net]]
- [[TensorFlow - Elastic Net]]
- [[Elastic Net Implementation Comparison]]

## References

- Zou, H., and Hastie, T. (2005). *Regularization and Variable Selection via the Elastic Net*.
- Friedman, J., Hastie, T., and Tibshirani, R. (2010). *Regularization Paths for Generalized Linear Models via Coordinate Descent*.

