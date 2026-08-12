---
type: comparison
name: Lasso Regression Implementation Comparison
compares:
  - "[[scikit-learn - Lasso]]"
  - "[[statsmodels - Lasso via fit_regularized]]"
  - "[[NumPy - Lasso Regression]]"
  - "[[SciPy - Lasso Regression]]"
  - "[[PyTorch - Lasso Regression]]"
  - "[[TensorFlow - Lasso Regression]]"
status: reviewed
tags:
  - comparison
  - lasso
---

# Lasso Regression Implementation Comparison

## Comparison Scope

This table compares the Big 6 package routes for [[Lasso Regression]]. A native estimator, a lower-level numerical building block, and a custom differentiable implementation are not equivalent levels of abstraction.

| Property | scikit-learn | statsmodels | NumPy | SciPy | PyTorch | TensorFlow |
|---|---|---|---|---|---|---|
| Support level | Native estimator | Native regularized fitting route | Custom construction; no native estimator | Custom construction; no dedicated estimator | Custom differentiable/subgradient implementation | Custom differentiable/subgradient implementation |
| Primary API | sklearn.linear_model.Lasso | statsmodels.OLS.fit_regularized | Array operations in a custom coordinate/proximal solver | scipy sparse operations plus a custom proximal solver | torch.nn.Linear plus explicit L1 penalty | tf.keras.regularizers.L1 |
| Fitting style | Coordinate descent with dual-gap stopping | Elastic-net interface with pure L1 setting | Coordinate descent or proximal gradient | Proximal gradient, coordinate descent, or generic constrained reformulation | Iterative autograd optimization | Keras iterative training |
| Solver route | Coordinate descent | Coordinate-descent-like regularized route | User-authored numerical algorithm | User-selected optimization routine | Chosen optimizer | Chosen Keras optimizer |
| Statistical inference | Limited | Limited after selection | None automatic | None automatic | None automatic | None automatic |
| Sparse support | Sparse coefficients; input support route-dependent | Limited | Manual | Yes for custom operations | Weights may become small but not reliably exact-zero | Small weights, not guaranteed exact-zero |
| GPU | No standard route | No | Normally CPU | Normally CPU | Yes | Yes |
| Representative training time | O(Tnp) dense high-level | O(Tnp) high-level | O(Tnp) dense | O(T nnz(X)) sparse high-level | O(Tnp) | O(Tnp) |
| Representative space | O(np + p) | O(np + p) | O(np + p) | O(nnz(X) + p) | O(bp + p), plus optimizer state | O(bp + p), plus optimizer state |
| Critical caveat | Feature scaling is essential; convergence and coefficient paths depend on tolerance, selection order, warm starts, and the exact alpha convention. | Intercept handling, refitting, trimming, and alpha scaling require explicit choices; ordinary post-selection standard errors are not automatically valid. | `numpy.linalg.lstsq` does not solve lasso; convergence requires a valid step size or correct coordinate updates and a stopping diagnostic. | A generic smooth `minimize` call is not a faithful treatment of the nondifferentiable L1 kink unless the problem is reformulated or a suitable method is supplied. | Generic gradient optimizers do not reproduce coordinate-descent soft thresholding and may not create exact zeros; proximal updates are needed for faithful sparsity. | Keras regularization adds a loss term but generic optimizers do not implement exact proximal thresholding; scaling conventions matter. |

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

## Package Notes

- [[scikit-learn - Lasso]]
- [[statsmodels - Lasso via fit_regularized]]
- [[NumPy - Lasso Regression]]
- [[SciPy - Lasso Regression]]
- [[PyTorch - Lasso Regression]]
- [[TensorFlow - Lasso Regression]]

## Complexity Warning

No single Big-O value applies across all six columns. First-order full-data iteration is commonly:

$$
O(Tnp)
$$

Sparse iterative work is commonly summarized as:

$$
O\left(T\operatorname{nnz}(X)\right)
$$

Dense coefficient-space curvature or factorization routes can require:

$$
O(np^2+p^3)
$$

time and:

$$
O(p^2)
$$

additional matrix storage. Bayesian posterior covariance, Newton Hessians, coordinate active sets, mini-batches, convergence tolerance, and backend precision create materially different costs.

## Decision Guide

- Choose scikit-learn for estimator pipelines and mature native predictive APIs when available.
- Choose statsmodels when its native statistical model and inferential results match the target.
- Choose NumPy for a small, explicit reference construction with manual responsibility.
- Choose SciPy for numerical algorithms, sparse operations, stable factorizations, or generic optimization building blocks.
- Choose PyTorch when the model belongs inside a differentiable or accelerator-enabled system.
- Choose TensorFlow when Keras integration, TensorFlow deployment, or accelerator execution is central.

Package choice does not change the mathematical identity of [[Lasso Regression]], but objective conventions and available inference can change the fitted result.

## References

- The six linked implementation notes.
- Official documentation for each named API or numerical building block.
- [[Lasso Regression]]

