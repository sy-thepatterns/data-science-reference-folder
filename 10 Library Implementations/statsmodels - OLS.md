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
