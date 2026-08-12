---
type: comparison
name: Ridge Regression Implementation Comparison
compares:
  - "[[scikit-learn - Ridge]]"
  - "[[statsmodels - Ridge via fit_regularized]]"
  - "[[NumPy - Ridge Regression]]"
  - "[[SciPy - Ridge Regression]]"
  - "[[PyTorch - Ridge Regression]]"
  - "[[TensorFlow - Ridge Regression]]"
status: reviewed
tags:
  - comparison
  - ridge
---

# Ridge Regression Implementation Comparison

## Comparison Scope

This table compares the Big 6 package routes for [[Ridge Regression]]. A native estimator, a lower-level numerical building block, and a custom differentiable implementation are not equivalent levels of abstraction.

| Property | scikit-learn | statsmodels | NumPy | SciPy | PyTorch | TensorFlow |
|---|---|---|---|---|---|---|
| Support level | Native estimator | Native regularized fitting route | Custom construction | Custom numerical construction | Custom differentiable implementation | Custom differentiable implementation |
| Primary API | sklearn.linear_model.Ridge | statsmodels.OLS.fit_regularized | numpy.linalg.solve or numpy.linalg.lstsq | scipy.linalg.solve, cho_factor/cho_solve, or scipy.sparse.linalg | torch.nn.Linear plus optimizer or torch.linalg | tf.keras.layers.Dense with L2 regularizer |
| Fitting style | Solver-dispatch estimator; dense, sparse, and iterative routes | Elastic-net interface with pure L2 setting | Direct linear-system or augmented least-squares solve | Direct dense or iterative sparse solve | Iterative autograd training or direct tensor solve | Keras iterative training |
| Solver route | auto selects among direct and iterative solvers | statsmodels regularized optimization | Linked LAPACK routine | LAPACK or sparse iterative method | Chosen optimizer or torch.linalg backend | Chosen Keras optimizer |
| Statistical inference | Limited | Regularized result inference is limited compared with OLS | None automatic | None automatic | None automatic | None automatic |
| Sparse support | Yes, solver-dependent | Limited | No in numpy.linalg | Yes through scipy.sparse | Operation-specific | Operation-specific |
| GPU | No standard route | No | Normally CPU | Normally CPU | Yes | Yes |
| Representative training time | Direct dense: O(np^2 + p^3); iterative: O(T nnz(X)) | Typically iterative; O(Tnp) high-level | O(np^2 + p^3) in coefficient space | Dense O(np^2 + p^3); sparse iterative O(T nnz(X)) | Iterative O(Tnp); direct O(np^2 + p^3) | O(Tnp) for full passes |
| Representative space | O(np + p^2) dense, route-dependent | O(np + p) | O(np + p^2) | Dense O(np + p^2); sparse route-dependent | Mini-batch O(bp + p), plus autograd state | O(bp + p), plus optimizer state |
| Critical caveat | The meaning of `alpha` follows scikit-learn's unnormalized residual-sum convention; solver choice changes complexity and numerical behaviour. | Exclude or separately weight the intercept if it should remain unpenalized; this route is not the same results object as ordinary OLS. | Do not penalize an intercept column unintentionally; an augmented least-squares formulation is often more stable than forming the Gram matrix. | `assume_a="pos"` is valid only when the penalized system is positive definite; augmented QR/SVD avoids squaring conditioning. | Optimizer `weight_decay` may have optimizer-specific semantics; exclude the bias explicitly when matching textbook ridge. | Keras regularizer scaling and reduction conventions must be matched to the mathematical objective; the bias is separate from the kernel. |

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
| $\lambda$ | Nonnegative penalty strength; larger values shrink coefficients more strongly. |
| $I$ | Identity matrix, with ones on its diagonal and zeros elsewhere. |
| $\lVert\beta\rVert_2^2$ | Sum of squared coefficients. |
| $z$ | Arbitrary nonzero direction used to test positive definiteness. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

If several features tell nearly the same story, ordinary least squares can give them huge positive and negative coefficients that cancel. Ridge charges for large coefficients, so it prefers a calmer solution that makes almost the same predictions without those wild cancellations.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Differentiate squared residual loss plus $\lambda\beta^T\beta$ and set the gradient to zero to obtain the ridge normal equations.
- Prove uniqueness for $\lambda>0$ by showing $z^T(X^TX+\lambda I)z=\lVert Xz\rVert_2^2+\lambda\lVert z\rVert_2^2>0$.
- Derive the augmented least-squares form by expanding the norm of the vertically stacked system.

## Package Notes

- [[scikit-learn - Ridge]]
- [[statsmodels - Ridge via fit_regularized]]
- [[NumPy - Ridge Regression]]
- [[SciPy - Ridge Regression]]
- [[PyTorch - Ridge Regression]]
- [[TensorFlow - Ridge Regression]]

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

Package choice does not change the mathematical identity of [[Ridge Regression]], but objective conventions and available inference can change the fitted result.

## References

- The six linked implementation notes.
- Official documentation for each named API or numerical building block.
- [[Ridge Regression]]

