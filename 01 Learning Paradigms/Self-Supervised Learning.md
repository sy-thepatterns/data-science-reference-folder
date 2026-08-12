---
type: learning-paradigm
name: Self-Supervised Learning
tasks:
  - "[[Representation Learning]]"
  - "[[Generative Modelling]]"
algorithms:
  - "[[Contrastive Learning]]"
  - "[[Masked Language Modelling]]"
  - "[[Autoencoder]]"
architectures: []
related:
  - "[[Unsupervised Learning]]"
  - "[[Supervised Learning]]"
  - "[[Transfer Learning]]"
status: complete
tags:
  - representation-learning
---

# Self-Supervised Learning

## Definition

Self-supervised learning creates supervisory targets from the structure of unlabelled data and uses them to learn representations or generative models.

## Notation

| Symbol | Meaning |
|---|---|
| $x$ | Original observation. |
| $t$ | Random transformation, masking, or view-generation rule. |
| $t(x)$ | Transformed view of $x$. |
| $s(x,t)$ | Target generated from $x$ and transformation $t$. |
| $f_\theta$ | Encoder or predictive model. |
| $g_\theta$ | Prediction or decoding head. |
| $z=f_\theta(x)$ | Learned representation. |
| $\ell$ | Pretext loss. |
| $\tau$ | Temperature parameter in many contrastive losses. |

## Intuition

A student can create practice questions from an unlabelled book: hide a word and predict it, compare two views of the same picture, or reconstruct a damaged passage. Solving these artificial tasks can teach useful structure, but only if the practice task rewards the knowledge needed later.

## Learning Signal

Targets come from masked, transformed, paired, ordered, or held-out parts of observations rather than manual labels.

## Main Settings

| Setting | Description |
|---|---|
| Contrastive | Bring representations of related views together and separate unrelated examples. |
| Reconstruction | Recover an input or hidden portion from a compressed or corrupted version. |
| Predictive | Predict future, missing, or contextual parts of the data. |
| Distillation | Match a slowly updated teacher or another view without external labels. |
| Generative | Model token, pixel, audio, or latent distributions. |

## Formal Setup

A general pretext objective samples an observation and transformation and minimizes

$$
\mathbb{E}_{x,t}
\left[
\ell\left(g_\theta(t(x)),s(x,t)\right)
\right]
$$

The transformation defines which information should be ignored and which should remain predictable. It is therefore part of the learning assumptions, not mere data plumbing.

## Typical Objectives and Strategies

- Masked prediction and denoising.
- Contrastive discrimination between positive and negative pairs.
- Reconstruction through an information bottleneck.
- Redundancy reduction or variance–covariance regularization.
- Teacher–student agreement across views.
- Autoregressive or latent-variable likelihood objectives.

## Data Structure and Splitting

The split must follow the unit expected to generalize: examples, people, entities, time periods, environments, or tasks. Preprocessing and model selection must use training data only. Any feedback-dependent collection process must be reproduced inside each training run rather than performed once using the full dataset.

## Main Tasks

- [[Representation Learning]]
- [[Generative Modelling]]
- Retrieval
- Pretraining for classification, regression, and sequence tasks

## Representative Algorithms

- Contrastive learning
- Masked language modelling
- [[Autoencoder]]
- Bootstrap/self-distillation methods
- Autoregressive pretraining

## Evaluation

- Evaluate downstream transfer with frozen probes and full fine-tuning; each answers a different question.
- Use identical downstream architecture, tuning, and compute budgets for baselines.
- Measure sample efficiency across several label budgets, not one endpoint.
- Evaluate retrieval, robustness, calibration, and subgroup performance in addition to average accuracy.
- Audit pretraining data for duplicates and overlap with downstream validation or test sets.
- Report pretraining data, compute, energy, augmentation rules, and sensitivity to random seeds.

## Strengths

### Reduced labelled-sample requirement

Pretraining estimates representation parameters from a large unlabelled sample before fitting a smaller labelled head. If the representation concentrates target-relevant variation into $d\ll p$ dimensions, downstream estimation variance can scale with $d/n_L$ rather than the raw dimension $p/n_L$.

### Invariance as variance reduction

When an augmentation $t$ preserves the target, enforcing $f(x)\approx f(t(x))$ averages over nuisance variation. This reduces within-class representation variance and can improve generalization from limited labels.

### Mutual-information retention

Contrastive objectives can be interpreted as encouraging representations that preserve information shared across valid views. The statistical benefit depends on the shared information overlapping with downstream signal; information unique to one view is deliberately discarded.

### Better-conditioned downstream optimization

A pretrained representation can place examples into a feature space where classes or responses are simpler. A linear probe then estimates fewer or better-separated parameters, lowering optimization difficulty and often estimator variance.

### Multi-task reuse

The same representation is estimated once from a large source corpus and reused across target tasks. Shared estimation cost and statistical strength are amortized across those tasks.

### Generative density information

Predictive and generative pretext tasks estimate aspects of $p(x)$ or conditional structure within $x$. This can support missing-part prediction, retrieval, and uncertainty even before target labels exist.

## Limitations and Failure Modes

### Pretext–target mismatch

The pretraining loss estimates structure in $p(x)$, whereas downstream risk depends on $p(y\mid x)$. There is no general theorem that a better estimate of source structure lowers target risk; nuisance factors may dominate the pretext objective.

### Unwanted invariance creates irreducible bias

If augmentation $t$ sometimes changes the true label, enforcing $f(x)=f(t(x))$ merges observations that should remain distinct. More unlabelled data then reinforces the wrong invariance rather than correcting it.

### Representation collapse

An objective based only on agreement can be minimized by $f(x)=c$ for every input. Collapse is visible statistically as near-zero feature variance or low covariance rank. Negative samples, stop-gradient mechanisms, or variance constraints are used to exclude this trivial solution.

### Shortcut variables

The representation maximizes predictability under the training distribution, so it may encode background, identity, acquisition device, or compression artifacts. High pretext accuracy can therefore reflect a confounder with no stable downstream relationship.

### Finite-corpus contamination

If downstream test examples or near duplicates occur in pretraining, the representation is no longer independent of the test set. The resulting transfer estimate is optimistically biased even though no test labels were used.

### Scale does not remove source bias

As unlabelled sample size grows, variance falls around the source distribution’s patterns, including demographic and measurement biases. More data reduces sampling error but not systematic bias.

### Compute-driven model selection

Large searches over data mixtures, augmentations, and checkpoints create winner’s-curse bias. Fair comparison requires equalized tuning effort and an untouched target test set.

### Linear-probe ambiguity

Frozen-probe performance measures linear accessibility, while full fine-tuning measures adaptability. Neither alone identifies all information stored in the representation, so conclusions depend on the evaluation protocol.

## Related Paradigms

- [[Unsupervised Learning]]
- [[Supervised Learning]]
- [[Transfer Learning]]

## References

- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*.
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*.
