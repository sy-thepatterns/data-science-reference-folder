---
type: vault-guide
status: complete
tags:
  - knowledge-management
---

# Property Schema

Properties are optional when they do not apply, but their names should remain consistent.

## Shared Properties

```yaml
---
type:
name:
aliases:
status: stub
tags: []
created:
updated:
sources: []
---
```

## Algorithm Properties

```yaml
---
type: algorithm
name:
aliases: []
learning_paradigm: []
task: []
family: []
foundations: []
objective: []
loss: []
optimization: []
solvers: []
metrics: []
implementations: []
introduced_in: []
related: []
applications: []
year:
status: stub
tags: []
---
```

## Implementation Properties

```yaml
---
type: implementation
name:
algorithm: []
library: []
language: []
backend: []
numerical_methods: []
hardware: []
version:
commit:
documentation:
source_code:
status: stub
tags: []
---
```

## Paper Properties

```yaml
---
type: paper
title:
authors: []
year:
venue:
doi:
url:
algorithms_introduced: []
datasets: []
metrics: []
status: stub
tags: []
---
```

## Source Stability

For implementation notes, I try to always record the library version and, where source code is examined, the commit or release tag. Implementation details may change even when the high level API remains stable.
