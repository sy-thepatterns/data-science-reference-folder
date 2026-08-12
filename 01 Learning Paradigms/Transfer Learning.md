---
type: learning-paradigm
name: Transfer Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
  - "[[Representation Learning]]"
algorithms:
  - "[[Fine-Tuning]]"
  - "[[Feature Extraction]]"
  - "[[Domain Adaptation]]"
architectures: []
related:
  - "[[Meta-Learning]]"
  - "[[Self-Supervised Learning]]"
  - "[[Supervised Learning]]"
status: complete
tags:
  - transfer-learning
---

# Transfer Learning

## Definition

Transfer learning reuses parameters, representations, data, or structural knowledge learned in a source setting to improve learning in a related target setting.

## Notation

| Symbol | Meaning |
|---|---|
| $\mathcal{D}_S,\mathcal{D}_T$ | Source and target domains or datasets. |
| $\mathcal{T}_S,\mathcal{T}_T$ | Source and target tasks. |
| $\theta_S$ | Parameters learned in the source setting. |
| $\theta_T$ | Parameters adapted for the target setting. |
| $f_{\theta_S}$ | Pretrained source model. |
| $L_T$ | Target-task loss. |
| $\lambda$ | Strength of a constraint keeping target parameters near source parameters. |
| $\phi(x)$ | Transferred feature representation. |

## Intuition

Learning to play piano can make learning another keyboard instrument easier because some skills transfer. It can also create bad habits if the new instrument works differently. Transfer learning asks which knowledge should be reused, which should be changed, and how much target evidence is needed to tell the difference.

## Learning Signal

A pretrained model, representation, source dataset, or source task plus target-domain feedback, which may be labelled or unlabelled.

## Main Settings

| Setting | Description |
|---|---|
| Feature extraction | Freeze a source representation and train a new target head. |
| Fine-tuning | Update some or all pretrained parameters on target data. |
| Domain adaptation | Reduce a difference between source and target input distributions. |
| Task transfer | Reuse knowledge when target labels or outputs differ. |
| Parameter-efficient tuning | Adapt a small set of added or selected parameters. |

## Formal Setup

Given source setting $(\mathcal{D}_S,\mathcal{T}_S)$ and target setting $(\mathcal{D}_T,\mathcal{T}_T)$, initialize or constrain target learning with source knowledge. One parameter-regularized form is

$$
\theta_T^\star
=
\arg\min_{\theta_T}
\left[
L_T(\theta_T;\mathcal{D}_T)
+
\lambda\lVert\theta_T-\theta_S\rVert_2^2
\right]
$$

This is one method, not the definition of the paradigm.

## Typical Objectives and Strategies

- Target loss after full or partial fine-tuning.
- Target loss using frozen source features.
- Distribution-alignment objectives for domain adaptation.
- Regularization toward source parameters or functions.
- Parameter-efficient adaptation with adapters, prompts, or low-rank updates.

## Data Structure and Splitting

The split must follow the unit expected to generalize: examples, people, entities, time periods, environments, or tasks. Preprocessing and model selection must use training data only. Any feedback-dependent collection process must be reproduced inside each training run rather than performed once using the full dataset.

## Main Tasks

- [[Classification]]
- [[Regression]]
- [[Representation Learning]]
- Detection, generation, and sequence modelling

## Representative Algorithms

- Feature extraction
- Fine-tuning
- Domain-adversarial adaptation
- Parameter-efficient fine-tuning
- Knowledge distillation

## Evaluation

- Compare with training from scratch and frozen-feature baselines under equal target-data and tuning budgets.
- Plot performance across target label budgets and adaptation compute.
- Use target validation and test sets that are independent of source pretraining data.
- Test several source checkpoints or domains to reveal sensitivity and negative transfer.
- Measure calibration, robustness, fairness, latency, memory, and catastrophic forgetting.
- Document source-data provenance, license, contamination risk, and representation gaps.

## Strengths

### Pretraining as an informative prior

A source estimate $\theta_S$ can regularize target parameters through $\lambda\lVert\theta_T-\theta_S\rVert^2$. When source and target optima are close, this reduces target estimator variance at the cost of only small bias.

### Lower target sample complexity

If pretraining supplies a representation with effective dimension $d$ instead of raw dimension $p$, target estimation error can depend on $d/n_T$ rather than $p/n_T$. The benefit is statistical only when the discarded directions are not target-relevant.

### Better optimization initialization

Starting near a broad basin of low target risk can reduce optimization error for a fixed compute budget. This is distinct from statistical generalization and should be measured with training and validation curves separately.

### Partial pooling across domains

Source knowledge and target evidence can be combined like hierarchical estimation. Weak target signals shrink more toward the source, while well-measured target directions can move farther away.

### Amortized source cost

The source representation is estimated once and reused. Across many target tasks, both computational cost and estimation effort are shared rather than repeated independently.

### Domain adaptation under overlap

When covariate shift holds, $p_S(y\mid x)=p_T(y\mid x)$, source observations can estimate target risk using weights $p_T(x)/p_S(x)$. Transfer can therefore use more data if density ratios are estimable and support overlaps.

## Limitations and Failure Modes

### Negative transfer as prior bias

If $\theta_S$ is far from the target optimum, strong regularization toward it introduces bias larger than the variance it removes. Negative transfer is the wrong side of the bias–variance trade-off.

### Support mismatch

Importance weighting cannot recover target regions where $p_S(x)=0$ but $p_T(x)>0$. No amount of source data identifies behavior in unsupported target regions without extra assumptions.

### Source bias becomes precise

Large source datasets reduce variance around source-specific social or measurement biases. Fine-tuning on a small target set may be statistically too weak to remove those well-estimated but inappropriate patterns.

### Contamination invalidates evaluation

If target test examples or duplicates occur in pretraining, the test statistic is no longer independent of fitting. Label-free exposure can still enable memorization and optimistic transfer estimates.

### Catastrophic forgetting

Target updates reduce target bias but may increase error on source capabilities. The trade-off can be viewed as minimizing target risk subject to a constraint on source-risk increase; unconstrained fine-tuning offers no guarantee.

### Hyperparameter selection with small targets

Layer freezing, learning rate, regularization, and checkpoint choice are tuned from limited target validation data. Selecting the best among many configurations produces high winner’s-curse bias.

### Calibration does not automatically transfer

Even if ranking features transfer, changes in class prevalence or conditional likelihood make source probabilities miscalibrated. Calibration must be estimated on representative target data.

### Unequal baseline budgets

A pretrained model incorporates far more data and compute than a scratch baseline. Claims of sample efficiency must state whether they concern target labels, total observations, total compute, or monetary cost.

## Related Paradigms

- [[Meta-Learning]]
- [[Self-Supervised Learning]]
- [[Supervised Learning]]

## References

- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*.
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*.
