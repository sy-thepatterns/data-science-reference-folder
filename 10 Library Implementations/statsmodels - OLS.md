---
type: implementation
name: statsmodels - OLS
algorithm:
  - "[[Linear Regression]]"
library:
  - "[[statsmodels]]"
backend:
  - "[[NumPy]]"
  - "[[SciPy]]"
hardware:
  - "[[CPU]]"
version: "Record installed version before source tracing"
status: developing
tags:
  - statistics
  - inference
  - cpu
---

# statsmodels - OLS

## Intended Use

`statsmodels.OLS` is oriented toward statistical modelling and inference. It exposes fitted coefficients together with standard errors, tests, confidence intervals, residual diagnostics, and model summaries.

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
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Read the formula as a recipe: identify what is observed, what must be learned, and what quantity says whether an answer is good. The notation compresses that recipe, but it does not turn the model into a solver or a software package.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Expand the stated objective from its definition and check that every term has compatible dimensions.
- Derive the first-order condition by differentiating or using a subgradient when the objective has a sharp corner.
- Check special cases and boundary values to confirm that the formula reduces to simpler known results.

## Public API

```python
import statsmodels.api as sm

X_with_intercept = sm.add_constant(X)
model = sm.OLS(y, X_with_intercept)
results = model.fit()

print(results.summary())
predictions = results.predict(X_new_with_intercept)
```

## Important Difference

An intercept is not automatically added to the design matrix by the base `OLS` constructor. Add a constant explicitly when the model requires one.

## Mathematical Objective

$$
\hat{\beta}
=
\arg\min_{\beta}
\lVert y-X\beta\rVert_2^2
$$

## Statistical Outputs

Depending on model configuration and covariance choice, results can include:

- coefficient estimates;
- standard errors;
- confidence intervals;
- t statistics;
- F statistics;
- residual diagnostics;
- goodness-of-fit measures;
- covariance estimators.

## Execution Trace

```text
statsmodels OLS
    ↓
design construction
    ↓
least-squares fitting
    ↓
covariance and inferential calculations
    ↓
NumPy / SciPy numerical routines
    ↓
BLAS / LAPACK
    ↓
CPU
```

## Complexity

The coefficient solve has dense least-squares complexity. Additional inferential calculations can require matrix products, covariance calculations, and residual passes.

## Best Use

Use this implementation when inferential outputs and diagnostics are central. Use [[scikit-learn - LinearRegression]] when estimator pipelines and prediction workflows are central.
