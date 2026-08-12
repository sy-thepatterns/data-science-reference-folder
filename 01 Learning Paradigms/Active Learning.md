---
type: learning-paradigm
name: Active Learning
tasks:
  - "[[Classification]]"
  - "[[Regression]]"
algorithms:
  - "[[Uncertainty Sampling]]"
  - "[[Query by Committee]]"
architectures: []
related:
  - "[[Supervised Learning]]"
  - "[[Online Learning]]"
  - "[[Semi-Supervised Learning]]"
status: complete
tags:
  - supervised
  - incremental
---

# Active Learning

## Definition

Active learning is a supervised-learning paradigm in which the learner chooses which examples should be labelled. Its purpose is to spend a limited annotation budget where an additional label is expected to improve the model most.

## Notation

| Symbol | Meaning |
|---|---|
| $\mathcal{L}$ | Current labelled set. |
| $\mathcal{U}$ | Current unlabelled pool. |
| $x,y$ | An input and its unknown or observed label. |
| $f_\theta$ | Predictive model with learned parameters $\theta$. |
| $a(x;\theta)$ | Acquisition score estimating the usefulness of labelling $x$. |
| $B$ | Maximum number or total cost of label requests. |
| $q$ | Query policy that selects examples for annotation. |
| $H[p_\theta(y\mid x)]$ | Predictive entropy, one possible uncertainty score. |
| $x^\star$ | Example selected by the acquisition rule. |
| $k$ | Number of examples requested together in a batch. |

## Intuition

Imagine studying with a teacher who will answer only 20 questions. Asking about facts you already understand wastes the budget. Active learning tries to find the questions whose answers would most change or improve what the model knows. The difficult part is that a confused model may not know what it does not know.

## Learning Signal

Labels are returned by an oracle after the learner requests them. The oracle may be a person, laboratory experiment, simulator, database lookup, or costly measurement.

## Main Settings

| Setting | Description |
|---|---|
| Pool-based | Choose queries from a fixed pool of unlabelled examples. |
| Stream-based | Accept or skip each arriving example before its label is known. |
| Membership-query | Construct a synthetic input and request its label, when such queries are valid. |
| Batch | Request several labels at once to reduce waiting and retraining overhead. |

## Formal Setup

Given labelled data $\mathcal{L}$, unlabelled pool $\mathcal{U}$, model $f_\theta$, and budget $B$, an acquisition policy repeatedly selects

$$
x^\star
=
\arg\max_{x\in\mathcal{U}}a(x;\theta)
$$

and asks the oracle for $y^\star$. The pair $(x^\star,y^\star)$ moves into $\mathcal{L}$, the model is updated, and the process continues until the budget or stopping rule is reached. Uncertainty sampling may use

$$
a(x;\theta)=H[p_\theta(y\mid x)]
$$

but uncertainty is only one acquisition strategy. The acquisition rule, predictive model, optimizer, implementation, and annotation system are separate components.

## Typical Objectives and Strategies

- Predictive uncertainty: query examples for which the model is least certain.
- Query by committee: query examples on which plausible models disagree.
- Expected model change: prefer labels expected to move the parameters or predictions most.
- Expected error reduction or information gain: estimate the downstream value of each possible label.
- Representativeness and diversity: cover the data distribution and avoid redundant batches.
- Cost-aware acquisition: maximize information or utility per unit of annotation cost.

## Data Structure and Splitting

The split must follow the unit expected to generalize: examples, people, entities, time periods, environments, or tasks. Preprocessing and model selection must use training data only. Any feedback-dependent collection process must be reproduced inside each training run rather than performed once using the full dataset.

## Main Tasks

- [[Classification]]
- [[Regression]]
- [[Ranking]]
- Structured prediction and scientific experimental design.

## Representative Algorithms

- [[Uncertainty Sampling]]
- [[Query by Committee]]
- Expected model change
- Expected error reduction
- Core-set and diversity-based selection
- Bayesian active learning

## Evaluation

- Plot performance against labels acquired, total annotation cost, and elapsed wall-clock time—not only training iterations.
- Compare with random sampling, stratified sampling, and a strong passive-learning baseline using the same initial seed set and budget.
- Repeat the full acquisition process across seeds; query choices make runs dependent and often highly variable.
- Keep the test set fixed, representative, and completely unavailable to the acquisition function.
- Measure calibration, class and subgroup coverage, oracle disagreement, duplicate-query rate, batch diversity, and computation per query.
- Evaluate the final deployment objective. A higher acquisition score is not itself evidence of better decisions.

## Strengths

### Label efficiency as a learning-curve improvement

Let $R(B)$ be expected test risk after purchasing $B$ labels. Active learning is statistically useful only when its learning curve falls faster than passive sampling:

$$
R_{\mathrm{active}}(B)<R_{\mathrm{passive}}(B)
$$

for budgets that matter operationally. The mechanism is not that every selected example is “hard”; it is that the conditional expected risk reduction per label is larger. Report the full risk-versus-budget curve and uncertainty across acquisition runs, because a single final score cannot establish label efficiency.

### Information-directed querying

Bayesian acquisition can select an input that maximizes expected information about parameters:

$$
a(x)=I(Y;\theta\mid x,\mathcal{L})
=
H(\theta\mid\mathcal{L})
-
\mathbb{E}_{Y\mid x,\mathcal{L}}
H(\theta\mid\mathcal{L},x,Y)
$$

This explains why a label can be valuable even when its predicted class is not maximally uncertain: it may distinguish between plausible models that imply different behavior elsewhere.

### Variance reduction near a decision boundary

For many classifiers, observations far from the decision boundary contribute little information about boundary parameters. Querying where the score is near zero can increase Fisher information in those directions and reduce estimator covariance, approximately $\mathcal{I}(\theta)^{-1}$. This benefit depends on the boundary being roughly correct; otherwise the method concentrates precision around the wrong region.

### Cost-aware statistical design

When label costs differ, the relevant quantity is not information alone but expected improvement per cost:

$$
x^\star
=
\arg\max_{x\in\mathcal{U}}
\frac{\mathbb{E}[\Delta R\mid x]}{c(x)}
$$

This can allocate scarce expert or laboratory effort more efficiently than treating all examples as equally expensive.

### Adaptive experimental design

In parametric scientific models, queries can be chosen to improve a design criterion such as reducing $\det\operatorname{Var}(\hat\theta)$ or its trace. Adaptivity lets later measurements target directions that remain weakly identified after earlier results, rather than committing the entire design before observing any outcomes.

### Rational stopping

The marginal value of another label can be compared with its marginal cost. If an estimated reduction $\mathbb{E}[\Delta R\mid\mathcal{L}]$ is smaller than the decision value assigned to that reduction, annotation can stop. This makes the budget a statistical decision rather than an arbitrary dataset size.

## Limitations and Failure Modes

### Selection bias and covariate distortion

The queried distribution $q(x)$ differs from the deployment distribution $p(x)$. An empirical average over queried data estimates risk under $q$, not $p$. Importance weighting can correct this in principle:

$$
\mathbb{E}_{p}[\ell]
=
\mathbb{E}_{q}
\left[
\frac{p(X)}{q(X)}\ell
\right]
$$

but small query probabilities create large weights and high variance. A representative holdout set is usually safer for calibration, prevalence, and evaluation.

### Uncertainty error drives acquisition error

Uncertainty sampling assumes the estimated distribution $\hat p(y\mid x)$ is calibrated enough to rank queries. If $\hat p$ is confidently wrong, entropy is low exactly where information is needed. Calibration error and epistemic uncertainty should therefore be evaluated separately; softmax confidence alone is not a reliable acquisition statistic.

### Cold-start variance

With a small seed set, parameter covariance and class-prevalence estimates are highly variable. Acquisition scores inherit this estimation noise, so early query rankings can be nearly random or systematically biased. Diverse random seeding reduces dependence on one unstable initial estimate.

### Oracle noise concentrates on hard cases

If queried labels have error probability $\eta(x)$, uncertainty sampling often selects regions where $\eta(x)$ is also high. For symmetric binary noise, the observed class signal is attenuated by a factor $1-2\eta$. As $\eta\to0.5$, the label contains almost no information, even though the example looks maximally uncertain.

### Batch dependence and redundant information

Selecting the top $k$ individual scores assumes additive value. Correlated queries violate this: $I(Y_1,Y_2;\theta)$ can be far smaller than $I(Y_1;\theta)+I(Y_2;\theta)$. Batch acquisition must estimate joint information or enforce diversity, otherwise it pays repeatedly for nearly the same evidence.

### Adaptive-data inference is nonstandard

The labelled sample depends on earlier outcomes and model states, so observations are not an ordinary independent random sample from the population. Classical standard errors and hypothesis tests can be biased unless the adaptive design and query propensities are incorporated.

### High variance across acquisition runs

A small difference in early labels can change every later query. This path dependence increases between-run variance, so one seed or one labelled trajectory substantially understates uncertainty.

### Computational cost can erase label savings

If each round scores $|\mathcal{U}|$ examples and retrains, acquisition work can grow roughly with the number of rounds times pool-scoring and training cost. Statistical efficiency per label must be reported alongside compute, latency, and expert turnaround.

## Related Paradigms

- [[Supervised Learning]]
- [[Online Learning]]
- [[Semi-Supervised Learning]]

## References

- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*.
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*.
