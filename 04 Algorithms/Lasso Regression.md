---
type: algorithm
name: Lasso Regression
aliases:
  - Lasso
  - Least Absolute Shrinkage and Selection Operator
learning_paradigm:
  - "[[Supervised Learning]]"
task:
  - "[[Regression]]"
family:
  - "[[Linear Models]]"
foundations:
  - "[[Linear Regression]]"
objective:
  - L1-regularized least squares
loss:
  - "[[Mean Squared Error]]"
optimization:
  - Convex nonsmooth optimization
solvers:
  - Coordinate Descent
  - Least-Angle Regression
  - Proximal Gradient Method
implementations:
  - "[[scikit-learn - Lasso]]"
  - "[[statsmodels - Lasso via fit_regularized]]"
  - "[[NumPy - Lasso Regression]]"
  - "[[SciPy - Lasso Regression]]"
  - "[[PyTorch - Lasso Regression]]"
  - "[[TensorFlow - Lasso Regression]]"
related:
  - "[[Ridge Regression]]"
  - "[[Elastic Net]]"
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

# Lasso Regression

## Overview

Lasso regression is a regularized version of [[Linear Regression]] that adds an absolute-value coefficient penalty. The geometry of the penalty can set coefficients exactly to zero, so fitting and feature selection occur together.

The intercept is normally unpenalized. Feature scaling is important because rescaling a feature changes the coefficient magnitude needed to represent the same prediction.

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
| $\lVert\beta\rVert_1$ | Sum of absolute coefficient values; the lasso penalty. |
| $\partial$ | Subdifferential: the set of valid slopes at a nondifferentiable point. |
| $S(z,\lambda)$ | Soft-thresholding operator, which moves $z$ toward zero and may set it exactly to zero. |
| $s=\lVert\hat\beta\rVert_0$ | Number of fitted nonzero coefficients; $\lVert\cdot\rVert_0$ is counting notation, not a true norm. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Picture each coefficient attached to a rubber band pulling it toward zero. The lasso's absolute-value band has a sharp corner at zero, so weak coefficients can stick there exactly. That is why lasso can remove features rather than merely making every coefficient smaller.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Derive the coordinate update by holding all other coefficients fixed and minimizing the resulting one-variable quadratic-plus-absolute-value problem.
- Use the subgradient optimality condition to prove the threshold rule for a zero coefficient.
- Explain sparsity geometrically by drawing elliptical loss contours touching the corners of an $L_1$ constraint diamond.

## Formal Definition

For:

$$
\lambda\ge 0
$$

lasso solves:

$$
\hat{\beta}_{\lambda}
=
\arg\min_{\beta}
\left\{
\frac{1}{2n}\lVert y-X\beta\rVert_2^2
+
\lambda\lVert\beta\rVert_1
\right\}
$$

where:

$$
\lVert\beta\rVert_1
=
\sum_{j=1}^{p}|\beta_j|
$$

Equivalent scaling conventions appear in software and literature. A regularization value must therefore be interpreted together with the exact objective convention.

## Optimality Conditions

The absolute-value penalty is not differentiable at zero. Its subgradient for coefficient:

$$
\beta_j
$$

is:

$$
\partial|\beta_j|
=
\begin{cases}
\{1\}, & \beta_j>0\\
[-1,1], & \beta_j=0\\
\{-1\}, & \beta_j<0
\end{cases}
$$

The optimum satisfies:

$$
0
\in
\frac{1}{n}X^T(X\hat{\beta}-y)
+
\lambda\partial\lVert\hat{\beta}\rVert_1
$$

For a zero coefficient, this permits:

$$
\left|
\frac{1}{n}x_j^T(y-X\hat{\beta})
\right|
\le
\lambda
$$

This threshold condition explains how predictors can be excluded exactly.

## Coordinate Update

With standardized features and partial residual:

$$
r_j
=
y-\sum_{k\ne j}x_k\beta_k
$$

a coordinate-descent update uses soft thresholding:

$$
\beta_j
\leftarrow
\frac{
S\left(\frac{1}{n}x_j^Tr_j,\lambda\right)
}{
\frac{1}{n}x_j^Tx_j
}
$$

where:

$$
S(z,\lambda)
=
\operatorname{sign}(z)
\max(|z|-\lambda,0)
$$

Coordinate descent is a solver for the lasso objective; it is not the lasso model itself.

## Statistical Properties

### Bias and Sparsity

Lasso shrinks nonzero coefficients and therefore introduces bias. It can reduce variance and produce a sparse representation when the signal is sufficiently concentrated.

### Correlated Predictors

When predictors are strongly correlated, lasso may select one and discard others in an unstable way. [[Elastic Net]] often behaves more smoothly for correlated groups.

### Identifiability

The fitted prediction may be unique under conditions weaker than those required for a unique coefficient vector. Exact uniqueness depends on the design matrix and active set.

### Model-Selection Consistency

Sparsity alone does not guarantee recovery of the true support. Consistent variable selection requires additional assumptions on the design, signal strength, and regularization sequence.

## Training Pseudocode

```text
INPUT:
    design matrix X
    target y
    penalty lambda
    convergence tolerance
    maximum iterations

1. Validate data and penalty.
2. Center and scale features using training data.
3. Initialize coefficients, commonly at zero or from a nearby path value.
4. Repeatedly update coefficients with the chosen convex solver.
5. Check an objective, dual-gap, or parameter-change criterion.
6. Recover the unpenalized intercept.

OUTPUT:
    sparse coefficient vector and intercept
```

## Complexity

Complexity depends on solver, sparsity, conditioning, tolerance, and warm starts. A dense coordinate sweep has cost approximately:

$$
O(np)
$$

With:

$$
T
$$

sweeps, a common upper-level description is:

$$
O(Tnp)
$$

Sparse implementations can depend instead on the nonzero count. Prediction for:

$$
s=\lVert\hat{\beta}\rVert_0
$$

active coefficients and a batch of size:

$$
m
$$

can cost:

$$
O(ms)
$$

when the active representation is exploited.

## Hyperparameters

| Parameter | Mathematical effect | Typical behaviour |
|---|---|---|
| Penalty strength | Controls shrinkage and sparsity | Larger values yield more zero coefficients |
| Tolerance | Defines numerical stopping | Tighter values require more iterations |
| Maximum iterations | Caps solver work | Too small a value can leave a nonconverged estimate |
| Feature scaling | Sets relative penalty across features | Standardization makes penalties comparable |

## Advantages

### Exact sparsity

The $L_1$ penalty has a nondifferentiable corner at zero. The optimality condition permits $\hat\beta_j=0$ whenever

$$
\left|\frac{1}{n}x_j^T(y-X\hat\beta)\right|\le\lambda
$$

so estimation and feature selection occur in one convex objective.

### High-dimensional estimation

Lasso can return a sparse solution when $p>n$. The penalty restricts the effective model even though ordinary least squares is nonunique, provided the design contains enough information about the sparse signal.

### Variance and storage reduction

Discarding weak coordinates can lower prediction variance and reduce the cost of storing or evaluating a model from $O(p)$ toward $O(s)$, where $s=\lVert\hat\beta\rVert_0$ is the active-feature count.

### Convex global objective

Squared loss plus $\lambda\lVert\beta\rVert_1$ is convex. Coordinate descent, proximal gradient, and related solvers target the same global optimum, although coefficient uniqueness can still depend on the design.

### Interpretable regularization path

As $\lambda$ decreases, variables enter or leave the active set. The path exposes the trade-off between validation error, sparsity, and coefficient magnitude rather than hiding selection behind a single fit.

### Useful when signal is sparse

When a small subset of standardized features contains most predictive information, the $L_1$ inductive bias can outperform dense unregularized or ridge estimates.

## Limitations

### Shrinkage bias

The same absolute-value penalty that creates zeros also subtracts magnitude from nonzero coefficients. Large true effects are biased toward zero, which can degrade estimation even when selected variables are correct.

### Correlated-feature instability

When predictors carry nearly interchangeable information, small perturbations may cause lasso to select different members of the group. Prediction can remain stable while the selected feature story changes.

### Selection requires strong assumptions

Recovering the true support is not guaranteed by sparsity alone. Consistent selection depends on signal strength, sample size, penalty sequence, noise, and design conditions such as restricted eigenvalue or irrepresentability assumptions.

### Scale dependence

Because $\lVert\beta\rVert_1$ penalizes coefficient units, features measured on larger numerical scales can be favored. Scaling rules must reflect both prediction and scientific meaning.

### Limited active-set size in some settings

For a generic design with $p>n$, a lasso solution commonly contains at most $n$ active predictors. This can be restrictive when the true signal is dense.

### Post-selection inference is nonstandard

Ordinary standard errors computed after selecting variables as if the model were prespecified ignore selection. Valid uncertainty requires selective-inference, debiasing, resampling, or an independent confirmation design.

### Squared-loss sensitivity remains

The residual term is still quadratic. The coefficient penalty does not bound the influence of large response residuals or high-leverage inputs.

## Failure Modes

### Unstable scientific conclusions

Different folds or bootstrap samples may select different correlated variables. Treating one active set as a uniquely discovered mechanism overstates the evidence.

### Penalty and preprocessing leakage

Scaling, imputation, feature filtering, and $\lambda$ selection must occur inside each training fold. Otherwise validation labels indirectly influence the fitted representation.

### Nonconvergence mistaken for sparsity

A loose tolerance or insufficient coordinate sweeps can leave coefficients inaccurately zero or nonzero. Solver diagnostics such as a duality gap distinguish optimization error from the statistical solution.

### Penalty convention mismatch

Different constants multiplying the loss or penalty change the numerical meaning of $\lambda$. Hyperparameters cannot be transferred safely without matching objectives.

### Rare-feature suppression

Standardization, low prevalence, or weak marginal correlation can cause rare but important predictors to be excluded, especially when their signal appears only through interactions not included in $X$.

### Outliers and shift

Extreme responses can distort the selected set, while changing correlations after deployment can make the chosen sparse proxy fail even if another correlated feature remains stable.

## Diagnostics

- Plot validation error and active-feature count over a regularization path.
- Verify solver convergence, preferably with a dual gap when available.
- Assess selection stability across resamples.
- Compare lasso with ridge and elastic net.
- Inspect residuals and influential observations.

## Related Algorithms

- [[Linear Regression]] is the unpenalized baseline.
- [[Ridge Regression]] shrinks continuously but rarely selects exactly.
- [[Elastic Net]] adds a squared penalty and can stabilize correlated groups.

## Implementations

- [[scikit-learn - Lasso]]
- [[statsmodels - Lasso via fit_regularized]]
- [[NumPy - Lasso Regression]]
- [[SciPy - Lasso Regression]]
- [[PyTorch - Lasso Regression]]
- [[TensorFlow - Lasso Regression]]
- [[Lasso Regression Implementation Comparison]]

## References

- Tibshirani, R. (1996). *Regression Shrinkage and Selection via the Lasso*.
- Hastie, T., Tibshirani, R., and Wainwright, M. *Statistical Learning with Sparsity*.
- [[Mean Squared Error]]

