---
title: Eval Set Format
weight: 6
description: The structure agentevals uses to define evaluation datasets, metadata, and scoring inputs.
---

Eval sets provide a repeatable way to organize the inputs and metadata used during evaluation.

## What an eval set is

An eval set typically describes:

- the items or examples being evaluated
- metadata associated with those items
- expected outputs, labels, or references when available
- which evaluators or metrics should be applied

This gives teams a stable structure for comparing results over time.

## Why it matters

A clear eval set format helps you:

- keep evaluation runs consistent
- compare changes across model or agent versions
- connect trace-derived behavior to dataset-level expectations
- share evaluation definitions across local, CI, and Kubernetes environments

## Practical guidance

When designing an eval set:

- keep identifiers stable
- store expected outputs or labels only when they are genuinely part of the task
- attach metadata that is useful for slicing results later
- avoid overloading one eval set with too many unrelated behaviors

## Related docs

- [Quick Start](/docs/quick-start/)
- [Custom Evaluators](/docs/custom-evaluators/)
- [OpenAI Evals API backend](/docs/openai-evals-api/)
- [Streaming](/docs/streaming/)
