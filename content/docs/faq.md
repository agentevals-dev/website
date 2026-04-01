---
title: FAQ
weight: 10
description: Frequently asked questions about agentevals.
---

## Does agentevals re-run my agent?

No. agentevals is built to score behavior from OpenTelemetry traces without re-running the agent.

## What kind of telemetry does agentevals use?

agentevals works from OpenTelemetry trace data emitted by your agent system. See [OTel Compatibility](/docs/otel-compatibility/) for more details.

## Can I write my own evaluators?

Yes. See [Custom Evaluators](/docs/custom-evaluators/).

## Can agentevals use external judging backends?

Yes. agentevals now includes an initial option to delegate evals to OpenAI's Evals API. See [OpenAI Evals API backend](/docs/openai-evals-api/).

## Can I deploy agentevals on Kubernetes?

Yes. The project now includes container deployment support and a Helm chart for Kubernetes. See [Kubernetes & Helm](/docs/kubernetes-helm/).

## Is agentevals only for batch processing?

No. There is also support for streaming-oriented workflows. See [Streaming](/docs/streaming/).
