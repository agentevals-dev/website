---
title: Integrations
weight: 4
description: How agentevals fits into trace pipelines, model backends, container deployments, and external evaluation systems.
---

agentevals is designed to sit on top of OpenTelemetry-based agent observability.

## Core integration points

### OpenTelemetry traces

agentevals evaluates from traces, so your primary integration surface is the telemetry your agent stack already emits.

For compatibility details and expectations around trace shape, see [OTel Compatibility](/docs/otel-compatibility/).

### Custom evaluators

If you want evaluation logic to stay inside your own Python code, use [Custom Evaluators](/docs/custom-evaluators/).

### OpenAI Evals API backend

agentevals includes an initial option to delegate eval execution to OpenAI's Evals API.

Use this when you want agentevals to remain your trace-oriented orchestration layer while an external backend performs judging.

See [OpenAI Evals API backend](/docs/openai-evals-api/).

### Containers and Kubernetes

For shared environments, CI systems, or cluster deployment, agentevals can run from a Docker image and can be installed on Kubernetes with the Helm chart.

See [Kubernetes & Helm](/docs/kubernetes-helm/).

### UI

The UI helps inspect results, compare runs, and review evaluation outputs visually.

See [UI Walkthrough](/docs/ui-walkthrough/).

## Typical integration patterns

- local development with Python package install
- CI evaluation jobs using a container image
- Kubernetes deployment via Helm
- trace ingestion from instrumented agent systems
- delegated rubric judging through OpenAI Evals API

## Choosing the right path

- Choose **custom evaluators** for code-first extensibility.
- Choose **OpenAI Evals API** when you want delegated judging.
- Choose **Docker/Kubernetes** when you need reproducible deployment.
- Choose **streaming** when you want near-real-time evaluation pipelines.
