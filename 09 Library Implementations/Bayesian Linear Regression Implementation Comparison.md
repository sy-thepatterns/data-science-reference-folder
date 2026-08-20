---
type: comparison
name: Bayesian Linear Regression Implementation Comparison
compares:
  - "[[scikit-learn - BayesianRidge]]"
  - "[[statsmodels - Bayesian Linear Regression]]"
  - "[[NumPy - Bayesian Linear Regression]]"
  - "[[SciPy - Bayesian Linear Regression]]"
  - "[[PyTorch - Bayesian Linear Regression]]"
  - "[[TensorFlow - Bayesian Linear Regression]]"
status: reviewed
tags:
  - comparison
  - bayesian
---

# Bayesian Linear Regression Implementation Comparison

## Comparison Scope

This table compares the Big 6 package routes for [[Bayesian Linear Regression]]. A native estimator, a lower-level numerical building block, and a custom differentiable implementation are not equivalent levels of abstraction.

| Property | scikit-learn | statsmodels | NumPy | SciPy | PyTorch | TensorFlow |
|---|---|---|---|---|---|---|
| Support level | Native empirical-Bayes estimator | Custom conjugate construction using statsmodels-adjacent arrays/results | Custom exact conjugate implementation | Custom exact conjugate implementation | Custom probabilistic construction | Custom probabilistic construction in core TensorFlow |
| Primary API | sklearn.linear_model.BayesianRidge | No general core Bayesian linear-regression estimator | numpy.linalg solve/cholesky plus random generation | scipy.linalg plus scipy.stats distributions | torch.distributions plus torch.linalg or custom variational model | TensorFlow tensor/linalg operations; TensorFlow Probability is separate |
| Fitting style | Iterative evidence maximization for isotropic coefficient and noise precisions | User-authored posterior algebra | Posterior precision and natural-parameter updates | Stable factorizations and probability-distribution utilities | Exact conjugate tensor algebra or approximate inference loop | Exact tensor algebra or user-authored approximate inference |
| Solver route | MacKay/Tipping-style fixed-point updates with dense linear algebra | NumPy/SciPy linear algebra | Linked LAPACK routines | Cholesky/triangular solves or other LAPACK routes | torch.linalg or chosen optimizer | tf.linalg or chosen optimizer |
| Statistical inference | Predictive standard deviation and coefficient covariance | Manual posterior and predictive summaries | Manual but exact under conjugacy | Manual posterior and predictive distributions | Manual; Pyro is a separate package | Manual in core TensorFlow |
| Sparse support | Dense route | Limited | No in numpy.linalg | Possible with sparse solvers | Operation-specific | Operation-specific |
| GPU | No | No | Normally CPU | Normally CPU | Yes | Yes |
| Representative training time | Approximately O(T(np^2 + p^3)) in coefficient space | Dense conjugate O(np^2 + p^3) | O(np^2 + p^3) | Dense O(np^2 + p^3) | Exact dense O(np^2 + p^3); variational O(Tnp) plus sampling | Exact dense O(np^2 + p^3); iterative approximation O(Tnp) high-level |
| Representative space | O(np + p^2) | O(np + p^2) | O(np + p^2) | O(np + p^2) | O(np + p^2) exact, approximation-dependent | O(np + p^2) exact, approximation-dependent |
| Critical caveat | This is a specific Bayesian ridge model with hyperparameters estimated from data, not a general prior/likelihood programming interface. | statsmodels Bayesian mixed-model classes do not substitute for a general Bayesian linear-regression API; PyMC is a separate package. | Avoid explicit matrix inversion; posterior correctness depends on the exact prior and known/unknown noise model. | SciPy provides components, not a unified estimator; distribution parameterizations and factor orientation must be checked carefully. | Core PyTorch provides distributions and tensor operations but not a turnkey Bayesian linear-regression estimator or general inference engine. | TensorFlow Probability offers richer probabilistic tools but is a separate package and must not be silently counted as core TensorFlow. |

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
| $p(\theta)$ | Prior distribution: uncertainty about parameters before conditioning on the current data. |
| $p(\mathcal{D}\mid\theta)$ | Likelihood: probability model for the observed data when parameters are fixed. |
| $p(\theta\mid\mathcal{D})$ | Posterior distribution after combining prior and likelihood. |
| $\sigma^2$ | Observation-noise variance. |
| $m_0,S_0$ | Prior mean and covariance of the coefficient vector. |
| $m_n,S_n$ | Posterior mean and covariance after $n$ observations. |
| $x_\star,y_\star$ | A new feature vector and its not-yet-observed target. |
| $\mathcal{N}(m,S)$ | Gaussian distribution with mean $m$ and covariance $S$. |
| $\tau^2$ | Prior coefficient variance in an isotropic Gaussian prior. |
| $m$ | Number of new rows predicted at once, when distinguished from the $n$ training rows. |
| $T$ | Number of iterative optimization steps or sweeps. |
| $\operatorname{nnz}(X)$ | Number of stored nonzero entries in a sparse matrix $X$. |
| $O(\cdot)$ | Big-O growth rate; it describes scaling, not an exact runtime. |

## Intuition

Start with a range of plausible coefficient values instead of pretending you know one exact answer. Data shifts weight toward values that explain what you saw. Prediction then averages over the remaining possibilities, so uncertainty grows naturally when the data leave several explanations alive.

## Derivation or Proof

These are useful routes for checking why the main equations work:

- Derive Bayes' rule from the two factorizations of a joint distribution: $p(\theta,\mathcal{D})=p(\mathcal{D}\mid\theta)p(\theta)=p(\theta\mid\mathcal{D})p(\mathcal{D})$.
- Derive Gaussian conjugacy by expanding the log prior plus log likelihood and completing the square in $\beta$.
- Derive the posterior predictive mean and variance with the laws of total expectation and total variance.

## Package Notes

- [[scikit-learn - BayesianRidge]]
- [[statsmodels - Bayesian Linear Regression]]
- [[NumPy - Bayesian Linear Regression]]
- [[SciPy - Bayesian Linear Regression]]
- [[PyTorch - Bayesian Linear Regression]]
- [[TensorFlow - Bayesian Linear Regression]]

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

Package choice does not change the mathematical identity of [[Bayesian Linear Regression]], but objective conventions and available inference can change the fitted result.

## References

1. [scikit-learn — `BayesianRidge`](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.BayesianRidge.html). Package documentation — public API, objective, parameters, solver choices, attributes, and input support.
2. [scikit-learn linear-model source](https://github.com/scikit-learn/scikit-learn/tree/main/sklearn/linear_model). Official source — estimator implementation, preprocessing, solver dispatch, sparse paths, and tests.
3. [Pedregosa et al. — “Scikit-learn”](https://jmlr.org/papers/v12/pedregosa11a.html). Open implementation paper — API, NumPy/SciPy structures, compiled kernels, sparse inputs, and parallelism.
4. [Murphy — *Probabilistic Machine Learning: An Introduction*](https://probml.github.io/pml-book/book1.html). Open textbook — probability, statistics, supervised learning, optimization, linear algebra, and probabilistic modelling.
5. [Lindley and Smith — “Bayes Estimates for the Linear Model”](https://www.stat.cmu.edu/~brian/463-663/week11/bayeslin.pdf). Open paper copy — conjugate Bayesian linear models and hierarchical structure.
6. [PyTorch — `torch.nn.Linear`](https://docs.pytorch.org/docs/stable/generated/torch.nn.Linear.html). Package documentation — affine layer, shapes, parameters, initialization, dtypes, and devices.
7. [PyTorch optimizers](https://docs.pytorch.org/docs/stable/optim.html). Solver documentation — gradient-based optimizers, parameter groups, hooks, and implementation options.
8. [TensorFlow — `tf.keras.layers.Dense`](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense). Package documentation — affine layer, tensor shapes, initializers, regularizers, and activation.
9. [TensorFlow Keras optimizers](https://www.tensorflow.org/api_docs/python/tf/keras/optimizers). Solver documentation — gradient optimizers, schedules, clipping, accumulation, and serialization.
