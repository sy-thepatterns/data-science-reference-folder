---
type: comparison
name: Logistic Regression Implementation Comparison
compares:
  - "[[scikit-learn - LogisticRegression]]"
  - "[[statsmodels - Logit]]"
  - "[[NumPy - Logistic Regression]]"
  - "[[SciPy - Logistic Regression]]"
  - "[[PyTorch - Logistic Regression]]"
  - "[[TensorFlow - Logistic Regression]]"
status: reviewed
tags:
  - comparison
  - logistic
---

# Logistic Regression Implementation Comparison

## Comparison Scope

This table compares the Big 6 package routes for [[Logistic Regression]]. A native estimator, a lower-level numerical building block, and a custom differentiable implementation are not equivalent levels of abstraction.

| Property | scikit-learn | statsmodels | NumPy | SciPy | PyTorch | TensorFlow |
|---|---|---|---|---|---|---|
| Support level | Native classification estimator | Native statistical model | Custom construction | Custom objective using native numerical tools | Custom differentiable implementation | Custom differentiable implementation |
| Primary API | sklearn.linear_model.LogisticRegression | statsmodels.Logit or statsmodels.GLM with Binomial family | Array operations in custom likelihood optimizer | scipy.optimize.minimize and scipy.special.expit/log_expit | torch.nn.Linear and torch.nn.BCEWithLogitsLoss | tf.keras Dense plus BinaryCrossentropy |
| Fitting style | Regularized binary or multinomial optimization | Maximum-likelihood estimation with inferential results | User-authored gradient, Newton, or IRLS loop | Generic likelihood optimization | Mini-batch or full-batch iterative training | Keras iterative training |
| Solver route | LBFGS, liblinear, Newton, SAG, or SAGA | Newton or selected likelihood optimizer | numpy.linalg solve for Newton systems | BFGS, L-BFGS-B, Newton-CG, trust methods, or user choice | Chosen optimizer | Chosen Keras optimizer |
| Statistical inference | Limited | Extensive | Manual | Manual | None automatic | None automatic |
| Sparse support | Yes, solver/input dependent | Limited | No native sparse arrays | Possible with custom sparse operations | Operation-specific | Operation-specific |
| GPU | No standard route | No | Normally CPU | Normally CPU | Yes | Yes |
| Representative training time | First-order O(Tnp); Newton routes may use O(np^2 + p^3) | O(T(np^2 + p^3)) for dense Newton-like fitting | Gradient O(Tnp); Newton O(T(np^2 + p^3)) | O(Tnp) first-order; curvature routes higher | O(Tnp) for full passes | O(Tnp) for full passes |
| Representative space | Solver-dependent; Hessian routes can use O(p^2) | O(np + p^2) | O(np + p^2) for Newton | Solver-dependent | O(bp + p), plus optimizer state | O(bp + p), plus optimizer state |
| Critical caveat | Regularization is applied by default and solver/penalty compatibility matters; this differs from unregularized textbook MLE. | An intercept is normally added explicitly; separation and covariance assumptions require diagnostic attention. | Naive sigmoid and log calculations can overflow; use stable log-add-exp identities and handle separation. | SciPy does not supply a logistic-regression estimator object; probability stability, regularization, intercept, and inference remain the user's responsibility. | Use logits directly for numerical stability; thresholding, calibration, and coefficient inference are separate concerns. | A sigmoid activation plus `from_logits=True` is incorrect; keep logits or set the loss convention consistently. |

## Package Notes

- [[scikit-learn - LogisticRegression]]
- [[statsmodels - Logit]]
- [[NumPy - Logistic Regression]]
- [[SciPy - Logistic Regression]]
- [[PyTorch - Logistic Regression]]
- [[TensorFlow - Logistic Regression]]

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

Package choice does not change the mathematical identity of [[Logistic Regression]], but objective conventions and available inference can change the fitted result.

## References

- The six linked implementation notes.
- Official documentation for each named API or numerical building block.
- [[Logistic Regression]]

