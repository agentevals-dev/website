---
title: UI Walkthrough
weight: 5
description: Explore agentevals results in the web UI.
---

The agentevals UI is the easiest way to inspect evaluation runs and understand how scores were produced.

## What the UI is for

Use the UI to:

- browse evaluation runs
- inspect individual trace-derived results
- compare scores across runs or datasets
- review metric outputs and evaluator judgments

## Typical workflow

1. run evaluations from your trace data
2. open the UI
3. select a run or dataset
4. inspect scores, failures, and trace-level context

## How it fits with newer features

The UI complements the newer v0.6.3 capabilities:

- if you deploy agentevals with containers or Kubernetes, the UI becomes easier to share across teams
- if you use delegated evaluation through OpenAI Evals API, the UI remains the place to review resulting outputs in the broader agentevals workflow
- if you use streaming, the UI helps inspect continuously arriving results

## Related docs

- [Quick Start](/docs/quick-start/)
- [Kubernetes & Helm](/docs/kubernetes-helm/)
- [OpenAI Evals API backend](/docs/openai-evals-api/)
- [Streaming](/docs/streaming/)
