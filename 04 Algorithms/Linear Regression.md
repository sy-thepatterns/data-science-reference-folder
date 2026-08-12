---
type: algorithm
name: Linear Regression
aliases:
  - Ordinary Least Squares Regression
  - OLS Regression
learning_paradigm:
  - "[[Supervised Learning]]"
task:
  - "[[Regression]]"
family:
  - "[[Linear Models]]"
foundations:
  - "[[Vector Space]]"
  - "[[Matrix Multiplication]]"
  - "[[Matrix Rank]]"
  - "[[Euclidean Norm]]"
  - "[[Expected Value]]"
  - "[[Variance]]"
objective:
  - "[[Least Squares]]"
loss:
  - "[[Mean Squared Error]]"
solvers:
  - "[[Normal Equations]]"
  - "[[QR Decomposition]]"
  - "[[Singular Value Decomposition]]"
implementations:
  - "[[scikit-learn - LinearRegression]]"
  - "[[statsmodels - OLS]]"
  - "[[NumPy - lstsq]]"
  - "[[SciPy - lstsq]]"
  - "[[PyTorch - Linear Regression]]"
  - "[[TensorFlow - Linear Regression]]"
related:
  - "[[Ridge Regression]]"
  - "[[Lasso Regression]]"
  - "[[Elastic Net]]"
applications:
  - "[[Continuous Outcome Prediction]]"
status: reviewed
tags:
  - linear
  - parametric
  - frequentist
  - convex
  - differentiable
  - interpretable
---

# Linear Regression

## Overview

Linear regression models a scalar response as a linear combination of input features. Ordinary least squares estimates the coefficients by minimizing the sum of squared residuals.

The word *linear* refers to linearity in the unknown coefficients. The input features themselves may include transformed quantities such as polynomials, interactions, or basis functions, provided the coefficients enter linearly.

## Complete Computational Pipeline

See [[Linear Regression Computational Pipeline]].

Vector spaces, projections, probability, and statistics
        ↓
Linear conditional-mean model
        ↓
Residual sum of squares / mean squared error
        ↓
Least-squares optimization
        ↓
Normal equations, QR, SVD, or an iterative sparse solver
        ↓
Coefficient estimate
        ↓
Library implementation
        ↓
BLAS / LAPACK or framework kernels
        ↓
CPU or GPU
        ↓
Continuous-value prediction and statistical inference

## Problem Definition

### Inputs

A design matrix:

$$
X \in \mathbb{R}^{n \times p}
$$

A response vector:

$$
y \in \mathbb{R}^{n}
$$

Optional sample weights:

$$
w_i \ge 0
$$

### Outputs

A coefficient vector:

$$
\hat{\beta} \in \mathbb{R}^{p}
$$

and, when used, an intercept:

$$
\hat{\beta}_0 \in \mathbb{R}
$$

Predictions for new rows $X_{\mathrm{new}}$ are:

$$
\hat{y}_{\mathrm{new}}
=
\hat{\beta}_0\mathbf{1}
+
X_{\mathrm{new}}\hat{\beta}
$$

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
| $\hat y$ | Vector of fitted or predicted responses. |
| $w_i$ | Optional nonnegative importance weight for example $i$. |
| $\mathbb{E}[\cdot]$ | Expected value under the stated probability model. |
| $\operatorname{Var}(\cdot)$ | Variance, measuring squared spread around an expectation. |
| $\lVert\cdot\rVert_2$ | Euclidean norm; its square sums squared entries. |
| $I$ | Identity matrix. |
| $\sigma^2$ | Error variance under a homoscedastic model. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Imagine fitting a flat sheet through a cloud of points. Each coefficient tilts the sheet in one feature direction. Least squares chooses the tilt that makes the combined vertical misses as small as possible, while the residuals are the arrows from the sheet to the observed points.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Expand $\lVert y-X\beta\rVert_2^2$, differentiate, and set the gradient to zero to obtain the normal equations.
- Use orthogonal projection to prove that the fitted vector lies in the column space of $X$ and the residual is perpendicular to that space.
- Prove convexity by showing the Hessian $2X^TX$ is positive semidefinite.

## Formal Statistical Model

Without writing the intercept separately, assume a column of ones is included in $X$

$$
y=X\beta+\varepsilon
$$

A common conditional-mean assumption is:

$$
\mathbb{E}[\varepsilon\mid X]=0
$$

Therefore:

$$
\mathbb{E}[y\mid X]=X\beta
$$

## Objective

Ordinary least squares solves:

$$
\hat{\beta}
=
\arg\min_{\beta}
\lVert y-X\beta\rVert_2^2
$$

Equivalently, it minimizes:

$$
\operatorname{RSS}(\beta)
=
\sum_{i=1}^{n}
\left(
y_i-x_i^{T}\beta
\right)^2
$$

or [[Mean Squared Error]], which differs only by the positive constant factor $1/n$.

## Derivation of the Normal Equations

Start with:

$$
L(\beta)
=
(y-X\beta)^{T}(y-X\beta)
$$

Expand:

$$
L(\beta)
=
y^{T}y
-
2\beta^{T}X^{T}y
+
\beta^{T}X^{T}X\beta
$$

Differentiate with respect to $\beta$:

$$
\nabla_\beta L
=
-2X^{T}y
+
2X^{T}X\beta
$$

At a stationary point:

$$
-2X^{T}y
+
2X^{T}X\hat{\beta}
=
0
$$

Therefore:

$$
X^{T}X\hat{\beta}
=
X^{T}y
$$

If $X$ has full column rank:

$$
\hat{\beta}
=
\left(X^{T}X\right)^{-1}X^{T}y
$$

This formula is primarily a mathematical expression. Numerical software should not usually form the inverse explicitly.

## Geometric Interpretation

The fitted vector:

$$
\hat{y}=X\hat{\beta}
$$

is the orthogonal projection of $y$ onto the column space of $X$.

The residual:

$$
r=y-X\hat{\beta}
$$

satisfies:

$$
X^{T}r=0
$$

Thus every residual vector is orthogonal to every column of the design matrix at the least-squares optimum.

## Convexity

The Hessian is:

$$
\nabla_\beta^2 L
=
2X^{T}X
$$

For every vector $z$:

$$
z^{T}X^{T}Xz
=
\lVert Xz\rVert_2^2
\ge 0
$$

Therefore $X^{T}X$ is positive semidefinite and the objective is convex.

If:

$$
\operatorname{rank}(X)=p
$$

then $X^{T}X$ is positive definite and the minimizer is unique.

## Assumptions

Distinguish predictive use from classical statistical inference.

### Required to Define and Fit OLS

- Numeric design matrix and target.
- Finite objective.
- A solver capable of handling the data shape and rank.

### Common Assumptions for Unbiased Conditional Estimation

- Correct linear conditional-mean specification.
- Zero conditional error mean:

$$
\mathbb{E}[\varepsilon\mid X]=0
$$

### Additional Classical Inference Assumptions

- Independent or appropriately modelled errors.
- Homoscedasticity for the simplest variance formula:

$$
\operatorname{Var}(\varepsilon\mid X)=\sigma^2 I
$$

- No exact multicollinearity for unique coefficients.
- Normality is not required to compute OLS coefficients; it is used for exact finite-sample Gaussian inference.

## Statistical Properties

### Unbiasedness

Under the fixed-design model and zero conditional error mean:

$$
\hat{\beta}
=
\beta
+
\left(X^{T}X\right)^{-1}X^{T}\varepsilon
$$

Taking the conditional expectation:

$$
\mathbb{E}[\hat{\beta}\mid X]
=
\beta
$$

### Variance

Under homoscedastic errors:

$$
\operatorname{Var}(\hat{\beta}\mid X)
=
\sigma^2
\left(X^{T}X\right)^{-1}
$$

### Gauss-Markov Property

Under the classical linear-model assumptions, OLS is the best linear unbiased estimator: among linear unbiased estimators, it has minimum variance in the positive semidefinite ordering.

### Sensitivity

Squared loss gives large residuals disproportionate influence, so OLS is sensitive to outliers and high leverage observations.

## Solvers

The statistical objective does not determine one unique computational algorithm.

### Normal Equations

- Complexity:

$$
O(np^2+p^3)
$$

- Can be efficient for small $p$.
- Less numerically stable because conditioning is squared.

### QR Decomposition

- Typical dense complexity:

$$
O(np^2)
$$

for $n\ge p$, ignoring lower-order terms.

- More stable than explicitly forming $X^{T}X$

### Singular Value Decomposition

- Shape-dependent dense complexity:

$$
O\left(
\min\left(np^2,n^2p\right)
\right)
$$

- Strong rank deficiency handling.
- Produces singular values and a minimum-norm solution.

### Iterative Sparse Solvers

For sparse matrices, methods such as LSQR avoid dense factorization. Their cost depends on:

$$
T = \text{number of iterations}
$$

and:

$$
\operatorname{nnz}(X)
=
\text{number of nonzero entries}
$$

A common dominant form is:

$$
O\left(T\operatorname{nnz}(X)\right)
$$

## Training Pseudocode

```text
INPUT:
    design matrix X
    target y
    fit_intercept flag
    optional sample weights
    chosen least-squares solver

1. Validate shapes and numeric values.
2. If fitting an intercept:
       compute feature and target offsets
       center data, or use an equivalent intercept treatment.
3. If sample weights are present:
       rescale rows by the square root of each weight.
4. Solve the resulting least-squares problem.
5. Store coefficients.
6. Recover the intercept from the offsets.

OUTPUT:
    coefficient vector
    intercept
    optional rank and singular-value diagnostics
```

## Prediction Pseudocode

```text
INPUT:
    new design matrix X_new
    fitted coefficients beta
    fitted intercept beta_0

1. Validate feature count.
2. Compute X_new @ beta.
3. Add beta_0.

OUTPUT:
    predicted target values
```

## Line-by-Line Complexity

Assume a dense matrix and one target.

| Step | Operation | Time | Additional space |
|---|---|---:|---:|
| Validation | Inspect matrix and target | $O(np)$ | Implementation-dependent |
| Centering | Compute means and subtract | $O(np)$ | $O(p)$ or a copied matrix |
| Weighting | Rescale rows | $O(np)$ | May copy $X$ |
| Factorization | QR or SVD-like dense solve | Shape-dependent | Shape-dependent |
| Intercept recovery | Dot product and subtraction | $O(p)$ | $O(1)$ |
| Prediction, one row | Dot product | $O(p)$ | $O(1)$ |
| Prediction, $m$ rows | Matrix-vector multiply | $O(mp)$ | $O(m)$ for output |

## Complexity Summary

There is no single solver-independent training complexity.

| Route | Typical dominant time | Additional notes |
|---|---:|---|
| Normal equations | $O(np^2+p^3)$ | Forms a $p\times p$ Gram matrix |
| Dense QR | $O(np^2)$ for $n\ge p$ | More stable than normal equations |
| Dense SVD | $O(\min(np^2,n^2p))$ | Rank revealing |
| Sparse iterative | $O(T\operatorname{nnz}(X))$ | Depends on convergence |
| Prediction, $m$ rows | $O(mp)$ | Dense model |

Input storage is:

$$
O(np+n)
$$

A dense copied design matrix may require another:

$$
O(np)
$$

## Hyperparameters

For textbook OLS there may be no regularization hyperparameter. Library implementations add procedural options.

| Parameter | Meaning | Mathematical effect | Computational effect |
|---|---|---|---|
| Fit intercept | Include a constant term | Changes hypothesis class | Requires centering or an added column |
| Solver tolerance | Numerical stopping or rank threshold | Can affect numerical rank or convergence | Trades accuracy for work in iterative methods |
| Positive coefficients | Constrain coefficients | Changes the optimization problem | Requires a constrained solver |
| Sample weights | Weighted residual objective | Changes each observation's influence | Requires row rescaling or weighted solver |

## Advantages

The advantages below are statistical claims about the estimator, not merely conveniences of a software implementation. For a new observation $(X_{\mathrm{new}},Y_{\mathrm{new}})$, the central quantity is expected prediction risk:

$$
R(\hat f)
=
\mathbb{E}
\left[
(Y_{\mathrm{new}}-\hat f(X_{\mathrm{new}}))^2
\right]
$$

Least squares is attractive when its low approximation bias and exactly characterized sampling variance make this risk competitive.

### Convex objective and global solutions

The ordinary least-squares objective

$$
L(\beta)=\lVert y-X\beta\rVert_2^2
$$

has Hessian $2X^TX$, which is positive semidefinite. Every local minimum is therefore global. When $X$ has full column rank, the Hessian is positive definite and the coefficient minimizer is unique.

### Transparent additive structure

The prediction $\hat y=\beta_0+x^T\beta$ separates into feature contributions $x_j\beta_j$. With the feature representation fixed, $\beta_j$ describes the change in the fitted response for a one-unit change in $x_j$ while the other represented features remain fixed. This is conditional interpretation, not automatically a causal effect.

### Projection geometry

The fitted vector is the orthogonal projection of $y$ onto the column space of $X$. At the optimum,

$$
X^T(y-X\hat\beta)=0
$$

so the residual is orthogonal to every fitted feature direction. This geometry makes residual behavior, leverage, rank, and nested-model comparisons easier to reason about.

### Strong statistical characterization

Under a correct conditional-mean model with $\mathbb{E}[\varepsilon\mid X]=0$, ordinary least squares is unbiased conditional on $X$. Under homoscedastic uncorrelated errors, its covariance is

$$
\operatorname{Var}(\hat\beta\mid X)=\sigma^2(X^TX)^{-1}
$$

which supports standard errors and confidence intervals when those assumptions and the inferential design are credible.

### Flexible basis representation

Linearity refers to the coefficients. Polynomial terms, splines, interactions, and domain features may be included in $X$ while the optimization remains linear in $\beta$. The model can therefore express nonlinear input–response shapes without becoming a nonlinear parameter-estimation problem.

### Efficient prediction and mature solvers

Dense prediction for $m$ rows costs $O(mp)$ and consists mainly of a matrix–vector product. The least-squares problem can be solved with QR, SVD, or sparse iterative methods chosen for the matrix shape, rank, sparsity, and precision requirements.

### Useful baseline

Because the assumptions and failure modes are comparatively visible, linear regression provides a demanding reference point. A more complex model should demonstrate improvement under the same leakage-safe split, metric, and deployment constraints.

### Finite-sample uncertainty is explicit

Conditional on a full-rank fixed design with homoscedastic errors,

$$
\hat\beta\sim\mathcal{N}\left(\beta,\sigma^2(X^TX)^{-1}\right)
$$

under Gaussian noise. Even without exact normality, this formula exposes how sample size, feature spread, and collinearity control uncertainty. Wider confidence intervals are a statistical consequence of weak information, not a solver defect.

## Limitations

These limitations arise when the assumptions connecting the sample estimator to the population target fail. They should be checked with residual structure, coefficient covariance, leverage, effective sample size, and out-of-sample risk rather than inferred from training fit alone.

### Restricted conditional mean

The model assumes that the conditional mean lies in the span of the supplied features:

$$
\mathbb{E}[Y\mid X]=X\beta
$$

If important curvature or interactions are absent from the basis, no solver can recover them. Adding features changes the model rather than merely improving the numerical solution.

### Squared-loss sensitivity

A residual $r_i$ contributes $r_i^2$ to the objective and $-2x_i r_i$ to the gradient. Its influence grows without bound as $|r_i|$ grows, so a small number of extreme responses can dominate the fit.

### Ill-conditioning and coefficient variance

When columns of $X$ are nearly dependent, the smallest singular value $\sigma_{\min}(X)$ approaches zero and

$$
\kappa(X)=\frac{\sigma_{\max}(X)}{\sigma_{\min}(X)}
$$

becomes large. Predictions within the observed feature subspace may remain adequate while individual coefficients become highly variable and sensitive to small perturbations.

### High-dimensional nonuniqueness

When $p>n$, full column rank is impossible. Many coefficient vectors can produce the same fitted values, so ordinary least squares alone does not identify a unique coefficient vector. A pseudoinverse selects a minimum-norm solution, while regularization introduces a different estimation criterion.

### Inference requires extra assumptions

Least-squares fitting itself does not require Gaussian errors, but familiar $t$ tests, $F$ tests, and naive standard errors rely on assumptions about dependence, variance, sampling, and model specification. Good predictive error does not validate those inferential assumptions.

### Weak extrapolation guarantees

Outside the observed feature support, the fitted hyperplane continues indefinitely. The algebra supplies a prediction but no evidence that the learned relationship remains stable there.

### Association is not intervention

The coefficient $\beta_j$ conditions on recorded features; omitted common causes, selection, measurement error, and treatment assignment can all prevent it from representing the effect of changing feature $j$.

## Failure Modes

### Exact rank deficiency

If $\operatorname{rank}(X)<p$, $X^TX$ is singular and the coefficient solution is nonunique. Explicit inversion fails; rank-revealing QR or SVD can still return a least-squares solution, but coefficient interpretation must acknowledge nonidentifiability.

### Near multicollinearity

Tiny singular values amplify noise and rounding. Coefficients may change sign or magnitude across small resamples even when aggregate predictions appear stable.

### Misspecified feature structure

Residual curvature, interactions, saturation, thresholds, or omitted variables indicate that $X\beta$ is not an adequate conditional-mean representation. More precise optimization cannot repair model misspecification.

### Heteroscedastic or dependent errors

If $\operatorname{Var}(\varepsilon\mid X)\ne\sigma^2I$, naive covariance formulas are wrong. Clustered, serially correlated, or repeated-measure errors require a design-appropriate covariance estimator or model.

### Influential observations

A point combining large residual magnitude with high leverage can move the fitted hyperplane substantially. Residual size alone is insufficient; leverage, Cook’s distance, and sensitivity refits reveal different aspects of influence.

### Target or preprocessing leakage

Using future information, target-derived features, duplicate entities across splits, or full-data scaling can make held-out error unrealistically small. Leakage changes the evaluation problem, not the mathematical estimator.

### Unsupported extrapolation and shift

Predictions may fail when covariate support, error variance, measurement procedures, or the conditional relationship changes after training. Monitoring must distinguish input drift from conditional and label shift.

## Related Algorithms

- [[Ridge Regression]] adds an $L_2$ penalty.
- [[Lasso Regression]] adds an $L_1$ penalty.
- [[Elastic Net]] combines $L_1$ and $L_2$ penalties.
- [[Huber Regression]] reduces sensitivity to large residuals.
- [[Logistic Regression]] is a classification model despite its name.
- [[Bayesian Linear Regression]] places probability distributions over parameters.

## Implementations

- [[scikit-learn - LinearRegression]]
- [[statsmodels - OLS]]
- [[NumPy - lstsq]]
- [[SciPy - lstsq]]
- [[PyTorch - Linear Regression]]
- [[TensorFlow - Linear Regression]]

## References

- [[Linear Regression Computational Pipeline]]
- [[Linear Regression Implementation Comparison]]
- [[Least Squares]]
- [[Mean Squared Error]]
