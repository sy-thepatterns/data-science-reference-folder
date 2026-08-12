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

## Notation

| Symbol | Meaning |
|---|---|
| $n$ | Number of observed examples or rows. |
| $p$ | Number of input features or columns. |
| $x_i$ | Feature vector for example $i$. |
| $y_i$ | Observed target or label for example $i$. |
| $X$ | Design matrix whose row $i$ is $x_i^T$; usually $X\in\mathbb{R}^{n\times p}$. |
| $y$ | Vector of all observed targets. |
| $\theta$ | Generic collection of parameters learned by a model. |
| $\ell$ | Loss assigned to a prediction and its observed target. |
| $\beta_0$ | Intercept: the prediction when all represented features are zero. |
| $\beta$ | Vector of $p$ coefficients; $\beta_j$ controls feature $j$ while other represented features are held fixed. |
| $\hat{\beta}$ | Estimated coefficient vector; a hat marks a quantity learned from data. |
| $X\beta$ | Vector of linear predictions before adding a separate intercept. |
| $\varepsilon$ | Unobserved error: the part of $y$ not represented by the linear mean model. |
| $r=y-X\beta$ | Residual vector: observed values minus fitted values. |
| $\lambda$ | Nonnegative overall penalty strength. |
| $\alpha$ | Mixing fraction: $1$ gives the lasso part and $0$ gives the ridge part under this convention. |
| $\lVert\beta\rVert_1$ | Sum of absolute coefficient values. |
| $\lVert\beta\rVert_2^2$ | Sum of squared coefficient values. |
| $\partial$ | Subdifferential used for the nondifferentiable absolute-value term. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Elastic net uses two kinds of pull on each coefficient. One can snap weak coefficients to exactly zero; the other smoothly keeps large or highly correlated coefficients from becoming unstable. The mixing value decides how much of each behaviour you want.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Recover lasso and ridge by substituting $\alpha=1$ and $\alpha=0$ into the objective.
- Prove convexity because squared loss, the $L_1$ norm, and the squared $L_2$ norm are convex and have nonnegative weights.
- Derive a coordinate update by combining soft thresholding with the extra quadratic shrinkage term.

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

Elastic net is statistically motivated when signal is partly sparse but distributed across correlated predictors. The $L_1$ term reduces effective dimension, while the $L_2$ term lowers variance along ill-conditioned directions. Its benefit is therefore a joint bias–variance and support-stability trade-off.

### Sparsity with strict curvature

Elastic net combines $L_1$ and squared $L_2$ penalties. The $L_1$ term can set coefficients to zero, while a positive $L_2$ component adds curvature and can make the coefficient solution unique even under correlated or rank-deficient designs.

### Grouping behavior

For similarly scaled, highly correlated predictors, the squared penalty discourages large differences between coefficients. Elastic net can retain a correlated group more readily than pure lasso, though it is not an explicit group-lasso objective.

### Continuum between lasso and ridge

Under the common parameterization, $\alpha=1$ recovers lasso and $\alpha=0$ recovers ridge. Intermediate values allow validation to trade exact sparsity against dense stability.

### High-dimensional applicability

The mixed penalty regularizes problems with $p>n$ and can select more than $n$ useful variables when the squared component stabilizes a dense correlated group.

### Convex objective

Squared loss, $\lVert\beta\rVert_1$, and $\lVert\beta\rVert_2^2$ are convex. With nonnegative weights their sum is convex, so specialized solvers target a global optimum.

### Practical regularization paths

Coordinate descent and warm starts efficiently trace solutions over $\lambda$ for a fixed mixing value, revealing changes in validation error and active-set size.

### Correlation changes the preferred penalty mix

When two standardized features have correlation near one, their separate effects are weakly identifiable. Pure lasso may select either feature with high sampling variability; adding $L_2$ curvature makes large coefficient differences costly and can reduce that selection variance, at the price of retaining more variables.

## Limitations

Because elastic net estimates both sparsity and shrinkage structure from finite validation data, uncertainty comes from the fitted coefficients and from selecting $(\lambda,\alpha)$. Similar validation risks can correspond to materially different active sets.

### Two-dimensional tuning

Both the overall strength $\lambda$ and mixing fraction $\alpha$ affect the estimand. Searching them increases validation variance, computation, and the risk of overfitting model selection.

### Coefficient bias

Both penalty components shrink coefficients. The $L_1$ part produces thresholding bias and the $L_2$ part shrinks every retained direction.

### No universal grouping guarantee

Correlated features are not always retained together. Grouping depends on correlation sign, scaling, noise, penalty mix, and other predictors.

### Scale and penalty-factor dependence

Changing feature units changes both penalty terms. Uniform standardization may itself be inappropriate for binary indicators, counts, or scientifically constrained coefficients.

### Interpretation depends on parameterization

Software packages distribute constants differently between loss, $L_1$, and $L_2$ terms. The same $(\lambda,\alpha)$ values can encode different objectives.

### Squared residual loss remains nonrobust

Mixed coefficient penalties do not bound residual influence. Large vertical errors and leverage points remain capable of distorting the fit.

## Failure Modes

### Endpoint collapse

A tuning procedure may choose $\alpha$ near zero or one, effectively reducing the method to ridge or lasso. That is not a defect, but claims about a special elastic-net benefit then lack support.

### Nested-validation leakage

Selecting two penalty dimensions using the final test set or preprocessing outside folds produces optimistic results and unstable hyperparameters.

### Unstable active sets

When several settings have similar validation risk, the selected features can vary substantially even if prediction changes little. Stability should be reported across resamples.

### Nonconvergence along the path

Warm starts help, but strong correlation and tight tolerances can require many sweeps. Incomplete convergence can contaminate model comparison across penalties.

### Mismatched feature groups

If correlated variables are redundant proxies rather than jointly useful measurements, retaining the group increases acquisition cost and can worsen interpretation.

### Outliers and deployment shift

Extreme residuals can alter both selection and shrinkage. Changes in feature correlation after deployment can also break a grouping pattern learned from training data.

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

