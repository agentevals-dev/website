---
title: OTel Compatibility
weight: 7
description: Understand what OpenTelemetry trace data agentevals expects and how compatibility affects scoring quality.
---

agentevals depends on OpenTelemetry traces as the source material for evaluation.

## Why compatibility matters

Evaluation quality depends on whether your traces contain the information needed to reconstruct meaningful agent behavior.

That often includes:

- clear span structure
- useful attributes and metadata
- model, tool, and response information
- enough context to connect a trace to an eval item or run

## What to check

Before relying on evaluation output, verify that your telemetry captures:

- the user request or task context
- intermediate model or tool actions when relevant
- final outputs
- identifiers needed to group or compare runs

## If your traces look different

Different frameworks emit different OTel conventions. In those cases:

- normalize where possible
- keep important identifiers stable
- verify that the extracted data matches the behavior you want to score

## Related docs

- [Quick Start](/docs/quick-start/)
- [Eval Set Format](/docs/eval-set-format/)
- [Custom Evaluators](/docs/custom-evaluators/)
- [Streaming](/docs/streaming/)
