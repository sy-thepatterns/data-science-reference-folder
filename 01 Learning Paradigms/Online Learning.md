---
type: learning-paradigm
name: Online Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
  - "[[Forecasting]]"
  - "[[Recommendation]]"
algorithms:
  - "[[Online Gradient Descent]]"
  - "[[Follow the Regularized Leader]]"
architectures: []
related:
  - "[[Supervised Learning]]"
  - "[[Active Learning]]"
  - "[[Reinforcement Learning]]"
status: complete
tags:
  - online
  - incremental
---

# Online Learning

## Definition

Online learning updates a predictor sequentially as observations or feedback arrive. The learner must act before seeing the current outcome and is judged over the sequence of decisions.

## Notation

| Symbol | Meaning |
|---|---|
| $t$ | Round index. |
| $T$ | Number of rounds. |
| $w_t$ | Parameters or decision selected at round $t$. |
| $\ell_t(w)$ | Loss function revealed or evaluated at round $t$. |
| $u$ | Comparator decision used to define regret. |
| $R_T(u)$ | Cumulative regret relative to $u$. |
| $\eta_t$ | Learning rate at round $t$. |
| $\nabla\ell_t(w_t)$ | Gradient of the current loss at $w_t$. |

## Intuition

Instead of studying a complete textbook before an exam, the learner answers one question, sees feedback, updates, and immediately faces the next question. It can adapt quickly, but yesterday’s lesson may become harmful if the world changes again.

## Learning Signal

At each round the learner predicts or acts, then receives full, partial, delayed, or censored feedback and updates its state.

## Main Settings

| Setting | Description |
|---|---|
| Full information | The complete loss function or outcome is observed after acting. |
| Bandit feedback | Only the consequence of the selected action is observed. |
| Streaming | Examples arrive once and may not be stored. |
| Continual or drifting | The relationship being learned changes over time. |

## Formal Setup

For rounds $t=1,\ldots,T$, choose $w_t$, incur $\ell_t(w_t)$, and compare against a fixed decision $u$:

$$
R_T(u)
=
\sum_{t=1}^{T}\ell_t(w_t)
-
\sum_{t=1}^{T}\ell_t(u)
$$

One possible optimizer is online gradient descent:

$$
w_{t+1}=w_t-\eta_t\nabla\ell_t(w_t)
$$

This update is an optimization algorithm, not the definition of online learning.

## Typical Objectives and Strategies

- Static regret against the best fixed comparator in hindsight.
- Dynamic regret against a changing comparator sequence.
- Low predictive loss under bounded memory and latency.
- Fast recovery after detected drift.
- Resource-aware objectives that include update time, storage, and communication.

## Data Structure and Splitting

The split must follow the unit expected to generalize: examples, people, entities, time periods, environments, or tasks. Preprocessing and model selection must use training data only. Any feedback-dependent collection process must be reproduced inside each training run rather than performed once using the full dataset.

## Main Tasks

- Streaming [[Classification]]
- Streaming [[Regression]]
- [[Forecasting]]
- [[Recommendation]] and advertising
- Monitoring and anomaly detection

## Representative Algorithms

- [[Online Gradient Descent]]
- [[Follow the Regularized Leader]]
- Passive-aggressive updates
- Exponentially weighted forecasters
- Online bagging

## Evaluation

- Evaluate prequentially: predict first, then score, then update.
- Plot rolling and cumulative loss, not only a final average.
- Measure recovery time after known or simulated drift.
- Compare with no-update, periodic-retraining, sliding-window, and oracle-changepoint baselines.
- Report memory, update latency, throughput, and delayed-label behaviour.
- Use chronological replay without allowing future features, labels, or normalization statistics to leak backward.

## Strengths

### Sublinear regret guarantees

For suitable convex losses, online algorithms can achieve $R_T(u)=O(\sqrt{T})$. Average regret then vanishes:

$$
\frac{R_T(u)}{T}=O(T^{-1/2})\to0
$$

so long-run average loss approaches that of the fixed comparator even without assuming independent observations.

### Statistical adaptation to recent data

Exponentially weighted risk, $\sum_{s\le t}\rho^{t-s}\ell_s$, gives recent observations more influence. The effective window is approximately $1/(1-\rho)$, explicitly trading variance from fewer effective samples against bias from stale data.

### Bounded-memory estimation

Sufficient-statistic or gradient updates can store $O(p)$ state rather than all $O(Tp)$ observations. This makes sequential estimation possible when retaining the full history is infeasible or disallowed.

### Prequential evaluation

Predict-then-update scoring estimates the actual sequential loss without training on the outcome being scored. It matches deployment timing more closely than a random split of temporally ordered data.

### Fast recovery under localized change

A sliding window or decayed estimator can discard obsolete evidence. When drift is real, accepting higher sampling variance from a smaller effective sample can reduce the larger bias caused by averaging across incompatible regimes.

### Adversarial robustness of guarantees

Regret bounds can hold for arbitrary loss sequences subject to stated convexity and boundedness conditions. This is stronger than relying entirely on an iid sampling assumption, although it is relative to a specified comparator.

## Limitations and Failure Modes

### Static regret can hide drift failure

Low regret against the best fixed $u$ says little when the optimal parameter changes. Dynamic regret depends on path variation such as $V_T=\sum_{t=2}^T\lVert u_t-u_{t-1}\rVert$. Rapid drift makes strong guarantees impossible without additional assumptions.

### Stability–plasticity trade-off

A large learning rate or short window reduces adaptation bias after change but increases estimator variance during stable periods. A small learning rate does the reverse. No fixed setting is optimal across all drift rates.

### Temporal dependence reduces effective sample size

Autocorrelation means $T$ observations contain less information than $T$ independent observations. For a stationary mean, a rough effective sample size is

$$
T_{\mathrm{eff}}
\approx
\frac{T}{1+2\sum_{k\ge1}\rho_k}
$$

so naive iid confidence intervals are too narrow.

### Delayed feedback

With delay $d$, updates use stale information and regret bounds typically worsen with total or maximum delay. The model may repeat a harmful decision many times before its consequence becomes observable.

### Feedback-induced selection bias

The system’s actions determine which labels are observed. For example, recommended items receive clicks while unshown items have missing counterfactual outcomes. Training directly on observed feedback estimates a policy-biased distribution.

### Catastrophic forgetting

Aggressive discounting reduces the statistical weight of older regimes. If an earlier regime returns, the model has high variance or bias because its evidence was discarded.

### Sensitivity to poisoning and bursts

Immediate updates give a corrupted burst high leverage before monitoring can react. Robust losses, clipping, checkpoints, and rollback are needed because asymptotic regret does not ensure safe transient behavior.

### Sequential tuning leakage

Choosing decay, alarms, or windows after examining the complete future stream uses unavailable information. Backtests must replay every tuning and update decision chronologically.

## Related Paradigms

- [[Supervised Learning]]
- [[Active Learning]]
- [[Reinforcement Learning]]

## References

- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*.
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*.
