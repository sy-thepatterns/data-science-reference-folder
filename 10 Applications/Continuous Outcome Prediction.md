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

## Notation

This note introduces no special mathematical symbols. Code identifiers, class names, routine names, and hardware names are literal technical names rather than algebraic variables.

## Intuition

This application is the real-world question at the top of the stack. Many different models can serve it, and the best mathematical score is not automatically the best practical decision unless costs, constraints, uncertainty, and people are considered.

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
