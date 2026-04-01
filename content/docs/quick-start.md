---
title: Quick Start
weight: 1
description: Install agentevals, run your first evaluation from OpenTelemetry traces, and learn where to go next.
---

agentevals scores AI agent behavior from OpenTelemetry traces without re-running the agent.

## Install

```bash
pip install agentevals
```

## What you need

To evaluate traces, you need:

- OpenTelemetry traces from your agent runs
- an eval configuration that defines which metrics or evaluators to run
- optional API keys if you use model-backed or delegated evaluators

## Basic workflow

The standard workflow is:

1. collect traces from your agent system
2. load those traces into agentevals
3. run built-in metrics, custom evaluators, or delegated backends
4. inspect results in the CLI or UI

agentevals is designed so evaluation happens from trace data rather than by replaying the original agent execution.

## Typical next steps

After installation, most teams go in one of these directions:

- **Define evals** using the [Eval Set Format](/docs/eval-set-format/)
- **Write custom logic** with [Custom Evaluators](/docs/custom-evaluators/)
- **Use delegated judging** with the [OpenAI Evals API backend](/docs/openai-evals-api/)
- **Run in production environments** with [Kubernetes & Helm](/docs/kubernetes-helm/)
- **Understand trace requirements** in [OTel Compatibility](/docs/otel-compatibility/)
- **Work with live data** using [Streaming](/docs/streaming/)
- **Inspect results visually** in the [UI Walkthrough](/docs/ui-walkthrough/)

## Deployment options

agentevals can be used in several ways:

- as a local Python package
- through container images
- on Kubernetes with the Helm chart

If you are deploying agentevals as a service, start with [Kubernetes & Helm](/docs/kubernetes-helm/).

## Choosing an evaluation approach

agentevals supports multiple ways to score behavior:

- **built-in metrics** for common trace-derived signals
- **custom evaluators** for project-specific logic
- **delegated evaluation backends** such as the initial OpenAI Evals API integration

For architecture guidance, see [Advanced](/docs/advanced/).
