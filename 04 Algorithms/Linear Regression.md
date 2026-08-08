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

Predictions for new rows $$X_{\mathrm{new}}$$ are:

$$
\hat{y}_{\mathrm{new}}
=
\hat{\beta}_0\mathbf{1}
+
X_{\mathrm{new}}\hat{\beta}
$$

## Notation

| Symbol | Meaning | Shape |
|---|---|---|
| $$n$$ | Number of observations | Scalar |
| $$p$$ | Number of features | Scalar |
| $$X$$ | Design matrix | $$n\times p$$ |
| $$y$$ | Target vector | $$n$$ |
| $$\beta$$ | Coefficient vector | $$p$$ |
| $$\beta_0$$ | Intercept | Scalar |
| $$\varepsilon$$ | Error vector | $$n$$ |
| $$\hat{y}$$ | Fitted values | $$n$$ |
| $$r$$ | Residual vector | $$n$$ |

## Formal Statistical Model

Without writing the intercept separately, assume a column of ones is included in $$X$$

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

or [[Mean Squared Error]], which differs only by the positive constant factor $$1/n$$.

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

Differentiate with respect to $$\beta$$:

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

If $$X$$ has full column rank:

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

is the orthogonal projection of $$y$$ onto the column space of $$X$$.

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

For every vector $$z$$:

$$
z^{T}X^{T}Xz
=
\lVert Xz\rVert_2^2
\ge 0
$$

Therefore $$X^{T}X$$ is positive semidefinite and the objective is convex.

If:

$$
\operatorname{rank}(X)=p
$$

then $$X^{T}X$$ is positive definite and the minimizer is unique.

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

- Can be efficient for small $$p$$.
- Less numerically stable because conditioning is squared.

### QR Decomposition

- Typical dense complexity:

$$
O(np^2)
$$

for $$n\ge p$$, ignoring lower-order terms.

- More stable than explicitly forming $$X^{T}X$$

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
| Validation | Inspect matrix and target | $$O(np)$$ | Implementation-dependent |
| Centering | Compute means and subtract | $$O(np)$$ | $$O(p)$$ or a copied matrix |
| Weighting | Rescale rows | $$O(np)$$ | May copy $$X$$ |
| Factorization | QR or SVD-like dense solve | Shape-dependent | Shape-dependent |
| Intercept recovery | Dot product and subtraction | $$O(p)$$ | $$O(1)$$ |
| Prediction, one row | Dot product | $$O(p)$$ | $$O(1)$$ |
| Prediction, $$m$$ rows | Matrix-vector multiply | $$O(mp)$$ | $$O(m)$$ for output |

## Complexity Summary

There is no single solver-independent training complexity.

| Route | Typical dominant time | Additional notes |
|---|---:|---|
| Normal equations | $$O(np^2+p^3)$$ | Forms a $$p\times p$$ Gram matrix |
| Dense QR | $$O(np^2)$$ for $$n\ge p$$ | More stable than normal equations |
| Dense SVD | $$O(\min(np^2,n^2p))$$ | Rank revealing |
| Sparse iterative | $$O(T\operatorname{nnz}(X))$$ | Depends on convergence |
| Prediction, $$m$$ rows | $$O(mp)$$ | Dense model |

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

- Simple and interpretable baseline.
- Convex objective.
- Closed-form characterization.
- Efficient prediction.
- Strong statistical theory.
- Supports uncertainty estimation in statistical implementations.

## Limitations

- Linear in coefficients and basis representation.
- Sensitive to outliers under squared loss.
- Coefficients can be unstable under multicollinearity.
- High-dimensional settings may require regularization.
- Predictive fit does not imply causality.

## Failure Modes

- Exact or near multicollinearity.
- Strong nonlinear structure not captured by features.
- Heteroscedasticity when using naive standard errors.
- Correlated errors.
- High-leverage influential observations.
- Extrapolation outside the observed feature range.
- Data leakage.
- Distribution shift.

## Related Algorithms

- [[Ridge Regression]] adds an $$L_2$$ penalty.
- [[Lasso Regression]] adds an $$L_1$$ penalty.
- [[Elastic Net]] combines $$L_1$$ and $$L_2$$ penalties.
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
