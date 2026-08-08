---
type: vault-guide
status: complete
tags:
  - knowledge-management
---

# Vault Conventions

## Note Types

I use one of the following `type` values:

- `algorithm`
- `algorithm-family`
- `learning-paradigm`
- `task`
- `architecture`
- `loss-function`
- `optimizer`
- `metric`
- `mathematical-foundation`
- `numerical-method`
- `library`
- `implementation`
- `paper`
- `dataset`
- `application`
- `hardware`
- `moc`
- `vault-guide`

## Status Values

| Status | Meaning |
|---|---|
| `stub` | Basic identity and links only |
| `developing` | Substantive but incomplete |
| `reviewed` | Mathematics, code, and references checked |
| `complete` | All applicable sections finished |

## Relationship Vocabulary

I also use these phrases consistently in prose and properties:

- `is-a`
- `variant-of`
- `regularized-version-of`
- `extends`
- `generalizes`
- `special-case-of`
- `depends-on`
- `uses`
- `optimizes`
- `minimizes`
- `implemented-by`
- `evaluated-by`
- `trained-on`
- `introduced-in`
- `related-to`
- `approximates`
- `executes-on`
- `calls`
- `backed-by`

## Naming

Theoretical notes use its names:

- `Linear Regression.md`
- `Singular Value Decomposition.md`
- `Mean Squared Error.md`

Implementation notes include the library and the names:

- `scikit-learn - LinearRegression.md`
- `statsmodels - OLS.md`
- `NumPy - lstsq.md`
- `PyTorch - Linear Regression.md`

## Tagging Rules

Folders, links, and properties carry the main taxonomy. Tags describe cross-cutting properties.

I use these property tags:

- `linear`
- `nonlinear`
- `parametric`
- `nonparametric`
- `probabilistic`
- `bayesian`
- `frequentist`
- `convex`
- `nonconvex`
- `differentiable`
- `nondifferentiable`
- `interpretable`
- `regularized`
- `kernel-method`
- `tree-based`
- `ensemble`
- `instance-based`
- `graph-based`
- `generative`
- `discriminative`
- `online`
- `incremental`
- `parallelizable`
- `distributed`
- `gpu-friendly`
- `sparse`
- `high-dimensional`

Application tags:

- `computer-vision`
- `nlp`
- `time-series`
- `bioinformatics`
- `recommendation`
- `speech`
- `robotics`

## Complexity Variables

I also try to define symbols before using them. Common variables include:

$$
n = \text{number of samples}
$$

$$
p = \text{number of features}
$$

$$
k = \text{number of classes or components}
$$

$$
T = \text{number of optimization iterations}
$$

$$
b = \text{batch size}
$$

As well as... never give one universal complexity when the result depends on the solver, data shape, sparsity, rank, precision, or stopping criterion.

## Source-Code Rule

Instead of pasting entire source files. I also record:

```text
Public API
    ↓
Internal function
    ↓
Computational operation
    ↓
Solver
    ↓
Backend
    ↓
Hardware
```

As this is a reference and not a comprehensive textbook, I tend to use short excerpts only when they are necessary to explain a specific operation.
