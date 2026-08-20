---
type: library
name: BLAS
language:
  - Compiled numerical library interface
status: developing
tags: []
---

# BLAS

## Notation

This note introduces no special mathematical symbols. Code identifiers, class names, routine names, and hardware names are literal technical names rather than algebraic variables.

## Intuition

Think of this library as a toolbox. It exposes convenient commands, but a command is not the mathematical idea it computes. Underneath, it may call a numerical routine, which may call a lower-level backend, which finally runs on hardware.

## Purpose

Standard low-level vector and matrix operations.

## Role in the Linear Regression Pipeline

## Numerical Backends

## Hardware Support

## Relevant Implementations

## Versioning Notes

Implementation behaviour must be recorded against a specific release or commit.

## Official References

## References

1. [Netlib BLAS](https://www.netlib.org/blas/). Official reference — Level 1, 2, and 3 interfaces and reference implementations.
2. [BLAS Technical Forum Standard](https://www.netlib.org/blas/blast-forum/). Open standard — routine semantics, data types, storage, and extended BLAS interfaces.
3. [OpenBLAS documentation](https://www.openmathlib.org/OpenBLAS/docs/). Open implementation documentation — optimized kernels, threading, architecture dispatch, and build configuration.
4. [SciPy Lecture Notes](https://scipy-lectures.org/). Open course notes — NumPy arrays, SciPy numerical routines, visualization, and scientific Python workflows.
