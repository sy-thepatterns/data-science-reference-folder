---
type: comparison
name: Huber Regression Implementation Comparison
compares:
  - "[[scikit-learn - HuberRegressor]]"
  - "[[statsmodels - RLM with HuberT]]"
  - "[[NumPy - Huber Regression]]"
  - "[[SciPy - Huber Regression]]"
  - "[[PyTorch - Huber Regression]]"
  - "[[TensorFlow - Huber Regression]]"
status: reviewed
tags:
  - comparison
  - huber
---

# Huber Regression Implementation Comparison

## Comparison Scope

This table compares the Big 6 package routes for [[Huber Regression]]. A native estimator, a lower-level numerical building block, and a custom differentiable implementation are not equivalent levels of abstraction.

| Property | scikit-learn | statsmodels | NumPy | SciPy | PyTorch | TensorFlow |
|---|---|---|---|---|---|---|
| Support level | Native scale-aware estimator | Native robust linear model | Custom construction | Custom objective from native building blocks | Custom differentiable implementation | Custom differentiable implementation |
| Primary API | sklearn.linear_model.HuberRegressor | statsmodels.RLM and statsmodels.robust.norms.HuberT | Array operations plus custom IRLS or gradient method | scipy.special.huber plus scipy.optimize.minimize | torch.nn.Linear and torch.nn.HuberLoss | tf.keras.layers.Dense and tf.keras.losses.Huber |
| Fitting style | Joint coefficient, intercept, and scale optimization with L2 penalty | M-estimation with robust scale and iteratively reweighted fitting | Residual weighting and repeated least-squares solves | Generic convex optimization | Iterative autograd training | Keras iterative training |
| Solver route | L-BFGS-B route | IRLS | numpy.linalg solve/lstsq inside IRLS | Chosen scipy.optimize method | Chosen optimizer | Chosen Keras optimizer |
| Statistical inference | Limited | Robust-model result summaries | None automatic | None automatic | None automatic | None automatic |
| Sparse support | Dense input route | Limited | No native sparse arrays | Possible with custom sparse operations | Operation-specific | Operation-specific |
| GPU | No | No | Normally CPU | Normally CPU | Yes | Yes |
| Representative training time | O(Tnp) high-level | O(T(np^2 + p^3)) for dense weighted solves | IRLS O(T(np^2 + p^3)) dense | O(Tnp) for first-order evaluations; solver-dependent | O(Tnp) | O(Tnp) |
| Representative space | O(np + p) | O(np + p^2) | O(np + p^2) | O(np + p) | O(bp + p), plus autograd state | O(bp + p), plus optimizer state |
| Critical caveat | This objective estimates scale and includes an L2 penalty; it is not identical to merely replacing MSE with a fixed-delta Huber loss. | Its scale, norm, covariance, and stopping definitions differ from scikit-learn's HuberRegressor. | Scale estimation, leverage robustness, stopping, and zero-residual handling must be implemented explicitly. | `scipy.special.huber` evaluates the loss only; it does not fit coefficients or estimate scale. | The built-in loss uses a fixed residual threshold; it does not reproduce joint scale estimation unless that is added. | This is fixed-delta loss training, not automatically the scale-aware scikit-learn or statsmodels estimator. |

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
| $r_i$ | Residual for example $i$: observed minus predicted value. |
| $\sigma$ | Positive residual scale used to make errors comparable. |
| $u_i=r_i/\sigma$ | Standardized residual. |
| $\delta$ | Positive cutoff between quadratic treatment of small errors and linear treatment of large errors. |
| $\rho_\delta$ | Huber loss. |
| $\psi_\delta$ | Derivative or score of the Huber loss. |
| $w_i$ | Robust weight assigned to example $i$ in an iteratively reweighted solver. |
| $R(\beta,\sigma)$ | Estimator-specific regularization or scale term. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Small mistakes are treated like squared error because they are useful for fine adjustment. Once a mistake is very large, Huber loss stops letting it dominate the lesson. It still counts the mistake, but its influence grows like a straight line instead of exploding like a square.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Check that the quadratic and linear pieces of $\rho_\delta$ meet with the same value and slope at $|u|=\delta$.
- Differentiate each piece to obtain the bounded score function $\psi_\delta$.
- Derive the iteratively reweighted form from $w_i=\psi(u_i)/u_i$ for nonzero residuals.

## Package Notes

- [[scikit-learn - HuberRegressor]]
- [[statsmodels - RLM with HuberT]]
- [[NumPy - Huber Regression]]
- [[SciPy - Huber Regression]]
- [[PyTorch - Huber Regression]]
- [[TensorFlow - Huber Regression]]

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

Package choice does not change the mathematical identity of [[Huber Regression]], but objective conventions and available inference can change the fitted result.

## References

- The six linked implementation notes.
- Official documentation for each named API or numerical building block.
- [[Huber Regression]]

