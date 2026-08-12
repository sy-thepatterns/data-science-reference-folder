---
type: learning-paradigm
name: Unsupervised Learning
tasks:
  - "[[Clustering]]"
  - "[[Density Estimation]]"
  - "[[Dimensionality Reduction]]"
  - "[[Anomaly Detection]]"
  - "[[Representation Learning]]"
algorithms:
  - "[[k-Means]]"
  - "[[Gaussian Mixture Model]]"
  - "[[Principal Component Analysis]]"
architectures: []
related:
  - "[[Self-Supervised Learning]]"
  - "[[Semi-Supervised Learning]]"
  - "[[Generative Modelling]]"
status: complete
tags:
  - unsupervised
---

# Unsupervised Learning

## Definition

Unsupervised learning seeks useful structure in observations without externally supplied target labels.

## Notation

| Symbol | Meaning |
|---|---|
| $\mathcal{D}=\{x_i\}_{i=1}^n$ | Dataset containing inputs but no externally supplied targets. |
| $x_i$ | Observation $i$. |
| $n$ | Number of observations. |
| $z_i$ | Latent representation or cluster assignment for observation $i$. |
| $\theta$ | Parameters of an unsupervised model. |
| $p_\theta(x)$ | Modelled probability density or mass. |
| $f_\theta(x)$ | Learned representation or transformation. |
| $d(x_i,x_j)$ | Chosen distance or dissimilarity between observations. |
| $k$ | Number of clusters or latent components, when used. |

## Intuition

Imagine receiving a box of mixed objects with no names attached. You can group similar objects, describe common patterns, compress them, or flag unusual ones—but the data alone cannot tell you which grouping a person will consider meaningful.

## Learning Signal

The empirical distribution, geometry, neighbourhoods, dependence, temporal order, or reconstruction structure of the observed inputs.

## Main Settings

| Setting | Description |
|---|---|
| Clustering | Organize observations into groups under a stated definition of similarity. |
| Density estimation | Estimate how probability is distributed over possible observations. |
| Dimensionality reduction | Represent data with fewer coordinates while preserving selected structure. |
| Latent-variable modelling | Explain observations using unobserved factors. |
| Anomaly detection | Identify observations inconsistent with a reference structure. |

## Formal Setup

Given

$$
\mathcal{D}=\{x_i\}_{i=1}^{n}
$$

fit a partition, density, representation, graph, or latent-variable model using an explicit objective. For example, maximum likelihood uses

$$
\hat\theta
=
\arg\max_\theta
\sum_{i=1}^{n}\log p_\theta(x_i)
$$

while clustering and reconstruction use different objectives and therefore define different notions of structure.

## Typical Objectives and Strategies

- Likelihood or score matching.
- Within-cluster distortion or graph cut criteria.
- Reconstruction error.
- Neighbourhood or manifold preservation.
- Sparsity, independence, or information bottlenecks.

## Data Structure and Splitting

The split must follow the unit expected to generalize: examples, people, entities, time periods, environments, or tasks. Preprocessing and model selection must use training data only. Any feedback-dependent collection process must be reproduced inside each training run rather than performed once using the full dataset.

## Main Tasks

- [[Clustering]]
- [[Density Estimation]]
- [[Dimensionality Reduction]]
- [[Anomaly Detection]]
- [[Representation Learning]]

## Representative Algorithms

- [[k-Means]]
- [[Gaussian Mixture Model]]
- [[Principal Component Analysis]]
- Autoencoders
- Spectral clustering

## Evaluation

- Use task-appropriate internal criteria, but do not confuse them with semantic usefulness.
- When labels exist only for evaluation, report external agreement without tuning directly on the test labels.
- Assess stability across seeds, samples, preprocessing choices, and hyperparameters.
- Evaluate downstream usefulness, reconstruction, likelihood, neighbourhood preservation, or anomaly-review yield as appropriate.
- Inspect outputs qualitatively with domain experts and document subjective decisions.
- Compare with simple baselines such as random projections, frequency models, or trivial partitions.

## Strengths

### Estimation without target labels

Likelihood and reconstruction objectives use every observed $x_i$, so parameter variance can decrease with large unlabelled $n$ even when $y$ is unavailable. The estimated object is structure in $p(x)$, not predictive relevance to an unknown target.

### Variance-preserving compression

Principal component analysis chooses a $d$-dimensional linear subspace maximizing captured sample variance, equivalently minimizing squared reconstruction error. The explained-variance ratio quantifies exactly how much second-moment structure is retained.

### Latent-variable regularization

Representing observations through a lower-dimensional $z$ reduces effective degrees of freedom. When the latent model is approximately correct, this can lower variance and improve denoising or missing-data estimates.

### Density-based anomaly scoring

A fitted $p_\theta(x)$ provides a principled notion of rarity through $-\log p_\theta(x)$. This separates the statistical score from the operational threshold and review policy.

### Exploratory hypothesis generation

Stable clusters, dependencies, or factors found across resamples can identify candidate structure for later labelled or confirmatory studies. Stability analysis makes this more defensible than interpreting one arbitrary fit.

### Pretraining signal

Structure learned from $p(x)$ can reduce downstream variance when it overlaps with target-relevant features. This benefit must be established by downstream evaluation rather than assumed from the unsupervised objective.

## Limitations and Failure Modes

### No target-identification guarantee

The marginal distribution $p(x)$ does not determine $p(y\mid x)$. The same observed inputs can support many incompatible labelings, so a cluster or representation cannot be declared semantically correct from unlabelled data alone.

### Nonidentifiability

Mixture labels can be permuted without changing likelihood, factor models can rotate latent axes, and different latent parameterizations can represent the same $p(x)$. Parameter interpretation requires extra constraints.

### Curse of dimensionality

Nonparametric density and neighborhood methods need rapidly increasing samples as dimension grows. Distance concentration makes nearest and farthest points similar, weakening geometric evidence unless data lie on lower-dimensional structure.

### Metric and scaling dependence

Clustering objectives such as $\sum_i\lVert x_i-\mu_{z_i}\rVert^2$ change when features are rescaled. The resulting groups reflect the analyst’s metric choices as much as the data.

### Optimization instability

Many objectives are nonconvex and have multiple local optima. Variation across initializations is part of estimator uncertainty, not merely a software inconvenience.

### Internal metrics can be gamed

Silhouette, reconstruction error, or likelihood measure the chosen mathematical criterion. A model can improve them while becoming less useful for a downstream semantic or decision task.

### Spurious structure from finite samples

Even noise has apparent clusters and principal directions in finite samples. Comparing with null models, held-out likelihood, or bootstrap stability is necessary before treating structure as real.

### Density misspecification

A high-dimensional model may assign high likelihood to observations humans consider abnormal or low likelihood to valid subgroups. Anomaly meaning depends on the reference population and decision cost, not density alone.

## Related Paradigms

- [[Self-Supervised Learning]]
- [[Semi-Supervised Learning]]
- [[Generative Modelling]]

## References

- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*.
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*.
