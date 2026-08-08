---
type: implementation
name: scikit-learn - LinearRegression
algorithm:
  - "[[Linear Regression]]"
library:
  - "[[scikit-learn]]"
language:
  - Python
backend:
  - "[[SciPy]]"
numerical_methods:
  - "[[Singular Value Decomposition]]"
  - sparse iterative least squares
hardware:
  - "[[CPU]]"
version: "1.9.0 documentation inspected on 2026-08-06"
documentation: "Official scikit-learn LinearRegression API"
status: reviewed
tags:
  - linear
  - cpu
---

# scikit-learn - LinearRegression

## Implements

[[Linear Regression]] using ordinary least squares for the default unconstrained problem.

## Public API

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression(
    fit_intercept=True,
    copy_X=True,
    tol=1e-6,
    n_jobs=None,
    positive=False,
)

model.fit(X, y)
predictions = model.predict(X_new)
```

## Important Parameters

| Parameter | Meaning | Mathematical effect | Computational effect |
|---|---|---|---|
| `fit_intercept` | Estimate an intercept | Adds a constant component | Triggers centering or equivalent preprocessing |
| `copy_X` | Permit or prevent reuse of input storage | None to the ideal objective | May change peak memory and mutation behaviour |
| `tol` | Sparse-solver tolerance | Controls iterative stopping for sparse input | Can affect iterations; no effect on dense least-squares path |
| `n_jobs` | Parallel jobs in supported cases | None | Helps mainly for multi-target sparse fits or positive constraints |
| `positive` | Require nonnegative coefficients | Changes to constrained least squares | Uses a different solver and supports dense arrays |

## Dense Execution Trace

```text
LinearRegression.fit
    ↓
data validation
    ↓
_preprocess_data
    ↓
optional sample-weight rescaling
    ↓
scipy.linalg.lstsq
    ↓
LAPACK least-squares driver
    ↓
BLAS / LAPACK implementation
    ↓
CPU
```

For dense unconstrained data, the estimator calls a dense least-squares routine rather than explicitly computing:

$$
\left(X^{T}X\right)^{-1}X^{T}y
$$

## Sparse Execution Trace

```text
LinearRegression.fit
    ↓
sparse-aware preprocessing
    ↓
intercept-adjusted linear operator
    ↓
scipy.sparse.linalg.lsqr
    ↓
repeated sparse matrix-vector products
    ↓
CPU
```

The `tol` value is passed to the sparse LSQR route.

## Positive-Coefficient Route

When `positive=True`, the problem is constrained:

$$
\hat{\beta}
=
\arg\min_{\beta}
\lVert y-X\beta\rVert_2^2
\quad
\text{subject to}
\quad
\beta_j\ge 0
$$

This is not identical to unconstrained ordinary least squares.

## Complexity

### Validation and Preprocessing

For dense:

$$
X \in \mathbb{R}^{n\times p}
$$

validation and centering require approximately:

$$
O(np)
$$

time.

If a full copy is made, additional memory can be:

$$
O(np)
$$

### Dense Solve

The dense solve is factorization-based, so a representative complexity is shape-dependent:

$$
O\left(
\min\left(np^2,n^2p\right)
\right)
$$

The precise constants and workspace depend on the SciPy/LAPACK driver and array layout.

### Sparse Solve

For:

$$
T=\text{LSQR iterations}
$$

the dominant cost is commonly expressed as:

$$
O\left(T\operatorname{nnz}(X)\right)
$$

plus vector operations.

### Prediction

For $$m$$ samples:

$$
O(mp)
$$

## Returned Attributes

- `coef_`: fitted coefficients.
- `intercept_`: fitted intercept.
- `rank_`: estimated rank for dense input.
- `singular_`: singular values for dense input.
- `n_features_in_`: number of features observed during fitting.

## Differences from a Textbook Derivation

- It does not normally form the explicit inverse.
- It has distinct dense, sparse, and nonnegative routes.
- It performs validation and intercept preprocessing.
- It supports sample weights.
- Dense fitting returns numerical-rank information.
- Runtime depends on linked SciPy and low-level numerical libraries.

## Best Use

Use this implementation for prediction-oriented tabular workflows, pipelines, preprocessing integration, cross-validation, and the broader scikit-learn estimator API.

For extensive coefficient inference, hypothesis tests, and regression diagnostics, compare [[statsmodels - OLS]].

## References

- Official scikit-learn `LinearRegression` documentation.
- Official scikit-learn source linked from the API page.
- Official SciPy dense and sparse least-squares documentation.
