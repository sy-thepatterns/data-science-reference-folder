---
type: learning-paradigm
name: Meta-Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
  - "[[Representation Learning]]"
algorithms:
  - "[[Model-Agnostic Meta-Learning]]"
  - "[[Prototypical Networks]]"
architectures: []
related:
  - "[[Transfer Learning]]"
  - "[[Supervised Learning]]"
  - "[[Online Learning]]"
status: complete
tags:
  - transfer-learning
---

# Meta-Learning

## Definition

Meta-learning learns across a collection or distribution of tasks so that a new related task can be learned with little data, computation, or interaction.

## Notation

| Symbol | Meaning |
|---|---|
| $\tau$ | One task. |
| $p(\tau)$ | Distribution from which tasks are sampled. |
| $\mathcal{D}^{\mathrm{sup}}_\tau$ | Support set used to adapt to task $\tau$. |
| $\mathcal{D}^{\mathrm{qry}}_\tau$ | Query set used to evaluate adaptation on task $\tau$. |
| $\phi$ | Meta-parameters shared across tasks. |
| $A$ | Adaptation procedure. |
| $\theta_\tau=A(\phi,\mathcal{D}^{\mathrm{sup}}_\tau)$ | Task-specific parameters after adaptation. |
| $L_\tau$ | Loss for task $\tau$. |

## Intuition

A person who has learned many related games can often understand a new game after seeing only a few examples. Meta-learning tries to learn that reusable starting knowledge or adaptation strategy, rather than learning each task from scratch.

## Learning Signal

A collection of tasks. Each training episode normally separates examples used for task adaptation from examples used to judge whether that adaptation worked.

## Main Settings

| Setting | Description |
|---|---|
| Optimization-based | Learn an initialization or update rule that adapts in a few steps. |
| Metric-based | Learn an embedding in which new examples can be classified by similarity. |
| Model-based | Use memory or recurrence to implement rapid adaptation. |
| Amortized inference | Learn a network that directly predicts task-specific parameters or distributions. |

## Formal Setup

For tasks $\tau\sim p(\tau)$, learn meta-parameters $\phi$ by minimizing performance after adaptation:

$$
\phi^\star
=
\arg\min_\phi
\mathbb{E}_{\tau\sim p(\tau)}
\left[
L_\tau^{\mathrm{qry}}
\left(
A(\phi,\mathcal{D}^{\mathrm{sup}}_\tau)
\right)
\right]
$$

The outer meta-objective, inner adaptation algorithm, base model, optimizer, and implementation remain separate layers.

## Typical Objectives and Strategies

- Fast adaptation after a fixed number of gradient steps.
- Accurate nearest-prototype or nearest-example decisions in a learned metric space.
- Low posterior predictive loss after inferring task-specific latent variables.
- Low cumulative regret when the new task is interactive.
- Robust adaptation across varying support-set sizes and label noise.

## Data Structure and Splitting

The split must follow the unit expected to generalize: examples, people, entities, time periods, environments, or tasks. Preprocessing and model selection must use training data only. Any feedback-dependent collection process must be reproduced inside each training run rather than performed once using the full dataset.

## Main Tasks

- Few-shot [[Classification]]
- Few-shot [[Regression]]
- [[Representation Learning]]
- Rapid control-policy adaptation

## Representative Algorithms

- [[Model-Agnostic Meta-Learning]]
- [[Prototypical Networks]]
- Matching Networks
- Reptile
- Learned optimizers

## Evaluation

- Split by entire tasks, not merely examples, so test tasks are genuinely unseen.
- Compare with transfer learning, multitask pretraining, and training from scratch under the same target-data budget.
- Report performance as a function of shots, classes, adaptation steps, and compute.
- Repeat task sampling and support/query sampling; both contribute variance.
- Test task-distribution shift rather than reporting only in-distribution episodes.
- Audit whether classes, entities, or source datasets leak across meta-train and meta-test tasks.

## Strengths

### Hierarchical shrinkage across tasks

Meta-learning can be interpreted as learning a shared prior or representation from tasks. In a hierarchical model, task parameters may satisfy $\theta_\tau\sim p(\theta\mid\phi)$. Sparse target data are then partially pooled toward $\phi$, reducing variance compared with estimating every $\theta_\tau$ independently.

### Lower few-shot estimation error

If tasks share a low-dimensional structure, learning that structure from many tasks reduces the number of free directions estimated from a new support set. The relevant sample size is not only examples per task but also the number and diversity of training tasks. Gains should appear as lower query risk for fixed shot count.

### Optimization learned from a task distribution

The meta-objective directly minimizes post-adaptation risk, $\mathbb{E}_\tau[L_\tau^{\mathrm{qry}}(A(\phi,\mathcal{D}_\tau^{\mathrm{sup}}))]$. This statistically favors initializations whose local parameter neighborhood contains many good task solutions, reducing adaptation bias after a small number of updates.

### Amortized inference

A learned mapping from support data to task parameters replaces repeated full estimation with one trained inference function. The initial meta-training cost is shared across future tasks, so average cost per task falls as the number of deployments grows.

### Uncertainty sharing

Probabilistic meta-learning can separate uncertainty shared across tasks from uncertainty specific to a new task. Posterior variance remains large when a support set is uninformative rather than forcing a confident point estimate.

### Task-aware evaluation target

Unlike ordinary pooled training, the estimand is explicitly performance after adaptation. This aligns training with few-shot deployment when support and query episodes reproduce the real task-generation process.

## Limitations and Failure Modes

### Task-distribution shift

The meta-objective estimates risk under $p_{\mathrm{train}}(\tau)$. Deployment instead incurs risk under $p_{\mathrm{test}}(\tau)$. If these differ, the generalization gap can grow with a divergence between the task distributions, and the learned adaptation bias can become negative transfer.

### Effective sample size is the task count

Thousands of episodes generated from a small number of underlying datasets or classes do not constitute thousands of independent tasks. Meta-generalization depends strongly on the number of genuinely independent task units; resampling the same units can create severe pseudo-replication.

### Nested-estimation variance

The query loss is evaluated after a support-dependent adaptation. Randomness in task sampling, support sampling, query sampling, initialization, and optimization all contribute variance. Confidence intervals that vary only examples within one episode are too narrow.

### Meta-overfitting

A high-capacity meta-learner can memorize recurring task identities or episode construction rules. The meta-training objective continues improving while held-out-task risk worsens, analogous to ordinary overfitting at the task level.

### Support-set noise is amplified

With $k$ shots, one corrupted label changes a fraction $1/k$ of the direct evidence and can strongly move the adapted model. Few-shot estimates have inherently high variance; robust adaptation and uncertainty reporting are essential.

### Baseline sensitivity

Observed gains depend on the comparator. Strong pretraining plus regularized fine-tuning may have similar bias and lower variance than a nested meta-learner. Equalizing data, augmentation, architecture, and compute is necessary for a statistical claim of improvement.

### Episode-definition dependence

Changing the number of classes, shots, query examples, or within-task balance changes the target risk. Results from one episodic design do not automatically estimate performance under another deployment design.

### Computational selection bias

Large meta-learning searches may report the best of many unstable configurations. Without nested validation or correction for repeated tuning, winner’s-curse bias makes the selected result optimistic.

## Related Paradigms

- [[Transfer Learning]]
- [[Supervised Learning]]
- [[Online Learning]]

## References

- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*.
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*.
