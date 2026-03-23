---
title: FAQ
weight: 6
description: Common questions about how agentevals works and how to deploy it.
---

## Does agentevals re-run my agent?

No. agentevals evaluates behavior from existing OpenTelemetry traces, so you can score what actually happened in production or staging without replaying requests.

## What do I need to get started?

You need:

- OpenTelemetry traces from your agent or workflow
- An evaluator model configured for scoring
- The CLI installed with `pip install agentevals-cli`

## Can I write my own evaluators?

Yes. Install the SDK with `pip install agentevals-evaluator-sdk` and register your Python evaluator class with the CLI.

## Where do results show up?

Results can be written back to your backend, exported in CI, or inspected in the agentevals UI.

## Does this work with any tracing backend?

It works anywhere you can access OpenTelemetry-compatible trace data or an OTLP endpoint that exposes the traces agentevals needs.

## Is there a web UI?

Yes — see the [UI Walkthrough](/docs/ui-walkthrough/) for the current workflow and screenshots.
