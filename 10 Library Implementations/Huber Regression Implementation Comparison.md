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

