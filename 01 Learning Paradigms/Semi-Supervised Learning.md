---
type: learning-paradigm
name: Semi-Supervised Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
algorithms:
  - "[[Pseudo-Labeling]]"
  - "[[Consistency Regularization]]"
  - "[[Label Propagation]]"
architectures: []
related:
  - "[[Supervised Learning]]"
  - "[[Unsupervised Learning]]"
  - "[[Self-Supervised Learning]]"
status: complete
tags:
  - semi-supervised
---

# Semi-Supervised Learning

## Definition

Semi-supervised learning combines a labelled dataset with additional unlabelled examples to improve a predictive model.

## Notation

| Symbol | Meaning |
|---|---|
| $\mathcal{L}$ | Labelled dataset. |
| $\mathcal{U}$ | Unlabelled dataset. |
| $L_{\mathrm{sup}}$ | Supervised loss on $\mathcal{L}$. |
| $L_{\mathrm{unsup}}$ | Constraint or proxy loss derived from $\mathcal{U}$. |
| $\lambda$ | Weight placed on the unlabelled-data term. |
| $f_\theta$ | Predictive model. |
| $\hat y$ | Pseudo-label predicted by a model. |
| $t(x)$ | Perturbed or augmented view of input $x$. |
| $\tau$ | Confidence threshold for accepting a pseudo-label, when used. |

## Intuition

Suppose a teacher labels a few photos while thousands remain unlabelled. The labelled examples explain what the categories mean; the unlabelled photos reveal which inputs are common and which examples look alike. This helps only when that structure lines up with the categories the teacher cares about.

## Learning Signal

True targets for labelled examples plus pseudo-label, consistency, density, graph, or representation constraints from unlabelled examples.

## Main Settings

| Setting | Description |
|---|---|
| Pseudo-labelling | Treat confident model predictions as temporary labels. |
| Consistency regularization | Require predictions to remain stable under meaningful perturbations. |
| Graph-based | Propagate labels across a similarity graph. |
| Generative | Model labelled and unlabelled observations jointly. |
| Transductive | Focus on the particular unlabelled examples available at training time. |

## Formal Setup

With labelled data $\mathcal{L}$ and unlabelled data $\mathcal{U}$, a common form is

$$
L(\theta)
=
L_{\mathrm{sup}}(\theta;\mathcal{L})
+
\lambda L_{\mathrm{unsup}}(\theta;\mathcal{U})
$$

The second term may encode consistency, entropy, graph smoothness, or pseudo-label agreement. These alternatives make different assumptions about the data.

## Typical Objectives and Strategies

- Supervised loss plus consistency under valid augmentations.
- Supervised loss plus confident pseudo-label loss.
- Graph smoothness between similar observations.
- Entropy minimization away from decision boundaries.
- Joint likelihood for observed inputs and available labels.

## Data Structure and Splitting

The split must follow the unit expected to generalize: examples, people, entities, time periods, environments, or tasks. Preprocessing and model selection must use training data only. Any feedback-dependent collection process must be reproduced inside each training run rather than performed once using the full dataset.

## Main Tasks

- [[Classification]]
- [[Regression]]
- Segmentation and structured prediction

## Representative Algorithms

- [[Pseudo-Labeling]]
- [[Consistency Regularization]]
- [[Label Propagation]]
- Teacher–student methods
- Semi-supervised generative models

## Evaluation

- Compare against the same model trained only on labelled data.
- Plot performance across labelled-set sizes and unlabelled-to-labelled ratios.
- Repeat labelled subset selection; conclusions can depend strongly on which examples receive labels.
- Test matched and mismatched unlabelled distributions, including unknown classes.
- Measure calibration, pseudo-label accuracy, acceptance rates, class coverage, and subgroup outcomes.
- Keep validation and test examples out of the unlabelled training pool unless the setting is explicitly transductive.

## Strengths

### Variance reduction under correct structure

Unlabelled data estimate the marginal geometry $p(x)$. If labels are smooth along that geometry, regularizing predictions across nearby inputs reduces the effective degrees of freedom and therefore estimator variance.

### Low-density boundary assumption

Entropy minimization and consistency methods favor decision boundaries in regions where $p(x)$ is small. When class clusters are separated by low-density gaps, this adds information about boundary location beyond the labelled sample.

### Pseudo-label effective sample size

Accepted pseudo-labels enlarge the training set. If pseudo-label error is small and sufficiently independent, parameter variance can decrease roughly with the enlarged effective sample size. Correlated or systematic pseudo-label errors provide far less benefit than their raw count suggests.

### Consistency as local regularization

A penalty such as $\lVert f_\theta(t(x))-f_\theta(x)\rVert^2$ discourages rapid changes along augmentation directions. This acts like a data-dependent smoothness prior and can improve stability from few labels.

### Graph-based information propagation

Label propagation uses a graph Laplacian penalty $f^TLf$, which is small when connected observations receive similar predictions. Labels can influence an entire well-connected component rather than only one point.

### Improved label efficiency is testable

The statistical claim is a lower target risk at a fixed labelled count $n_L$ after adding $n_U$ unlabelled examples. Learning curves over both $n_L$ and $n_U$ reveal whether gains come from unlabelled structure or merely extra tuning.

## Limitations and Failure Modes

### Unlabelled data help only under assumptions

Without a relationship between $p(x)$ and $p(y\mid x)$, unlabelled inputs provide no information about labels. Two labelings can share exactly the same $p(x)$ but require opposite decision boundaries.

### Confirmation bias from pseudo-label noise

If a model’s error rate among accepted pseudo-labels is $\eta$, the added training signal is label-contaminated. Retraining may increase confidence without reducing $eta$, creating dependent errors that do not average away like independent noise.

### Distribution mismatch

When unlabelled data follow $q(x)$ rather than target $p(x)$, the unsupervised term regularizes the wrong regions. Density-ratio correction is difficult when $p/q$ is large or support does not overlap.

### Confidence selection bias

Thresholding accepts examples with $\max_y\hat p(y\mid x)>\tau$. The resulting pseudo-labelled set overrepresents easy groups and majority classes. Its empirical class distribution is not an unbiased estimate of deployment prevalence.

### Class-imbalance amplification

If majority-class predictions are better calibrated, more majority pseudo-labels pass the threshold, which further improves that class and widens the gap. Per-class thresholds or distribution alignment add assumptions of their own.

### Invalid augmentations

Consistency assumes $Y(x)=Y(t(x))$. Violations introduce systematic label noise whose bias can increase with $n_U$ because the incorrect constraint is applied repeatedly.

### Transductive leakage

Using test inputs in the unlabelled pool estimates a different, transductive risk. Presenting that result as ordinary inductive generalization overstates how the method will handle genuinely new inputs.

### Hyperparameter-selection variance

The weight $\lambda$, confidence threshold, teacher decay, and augmentation strength are selected from limited labelled validation data. When labels are scarce, validation noise can dominate the apparent gain.

## Related Paradigms

- [[Supervised Learning]]
- [[Unsupervised Learning]]
- [[Self-Supervised Learning]]

## References

- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*.
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*.
