---
type: application
name: Continuous Outcome Prediction
domain: General
tasks:
  - "[[Regression]]"
algorithms:
  - "[[Linear Regression]]"
metrics:
  - "[[Mean Squared Error]]"
  - "[[R Squared]]"
status: developing
tags:
  - regression
---

# Continuous Outcome Prediction

## Problem Context

Estimate a quantitative target from observed features.

Examples include:

- cost estimation;
- duration prediction;
- concentration estimation;
- biological measurement prediction;
- demand estimation.

## Baseline

[[Linear Regression]] is often a useful baseline when a linear conditional relationship is plausible or when interpretability is important.

## Evaluation

Use held-out data and metrics appropriate to the consequence of errors. Mean squared error emphasizes large residuals, while absolute-error metrics are less sensitive to them.

## Risks

- Extrapolation
- Distribution shift
- Omitted-variable bias
- Data leakage
- Confusing association with causation
