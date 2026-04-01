---
title: Advanced
weight: 2
description: Advanced usage patterns for evaluation backends, deployment, trace compatibility, and scaling agentevals.
---

This guide summarizes the main advanced building blocks in agentevals and points to the deeper reference pages.

## Evaluation architecture

agentevals evaluates agent behavior from OpenTelemetry traces instead of replaying the agent.

Depending on your needs, you can combine:

- **built-in metrics** for fast trace-native scoring
- **custom evaluators** for Python-defined logic tailored to your app
- **delegated backends** when you want an external system to judge outputs

The initial delegated option is the [OpenAI Evals API backend](/docs/openai-evals-api/).

## Deployment patterns

agentevals can run:

- locally during development
- in containers for reproducible environments
- on Kubernetes using the project Helm chart

For cluster deployment details, configuration knobs, and install examples, see [Kubernetes & Helm](/docs/kubernetes-helm/).

## Trace model and compatibility

The quality of evaluation depends on the shape and completeness of your traces.

If your agent framework emits OpenTelemetry data with different conventions, review [OTel Compatibility](/docs/otel-compatibility/) to understand what agentevals expects and how to adapt inputs.

## Eval definitions

As eval setups grow, it helps to standardize how datasets, evaluators, and metadata are represented.

See [Eval Set Format](/docs/eval-set-format/) for the structure used by agentevals.

## Live and incremental processing

If you want to evaluate data continuously rather than in one batch, see [Streaming](/docs/streaming/).

## Extending agentevals

If built-in metrics are not enough, use [Custom Evaluators](/docs/custom-evaluators/) to implement project-specific scoring logic.

## Recommended reading order

For teams adopting newer v0.6.3 capabilities, this is a good progression:

1. [Quick Start](/docs/quick-start/)
2. [Eval Set Format](/docs/eval-set-format/)
3. [Custom Evaluators](/docs/custom-evaluators/)
4. [OpenAI Evals API backend](/docs/openai-evals-api/)
5. [Kubernetes & Helm](/docs/kubernetes-helm/)
6. [OTel Compatibility](/docs/otel-compatibility/)
7. [Streaming](/docs/streaming/)
