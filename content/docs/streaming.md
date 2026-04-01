---
title: Streaming
weight: 8
description: Run agentevals in streaming-oriented workflows for incremental or near-real-time evaluation.
---

Streaming support helps teams evaluate traces as they arrive instead of waiting for a single large batch.

## When streaming is useful

Use streaming workflows when you want to:

- monitor agent quality continuously
- reduce delay between trace ingestion and evaluation
- power operational dashboards or alerts
- support long-running systems with ongoing traffic

## Tradeoffs

Compared with pure batch processing, streaming workflows usually require more care around:

- ordering and completeness of traces
- idempotency
- partial results
- operational observability

## Good use cases

- production monitoring pipelines
- continuous staging environments
- shared internal evaluation services
- Kubernetes-based deployments where evaluation runs as a long-lived service

## Related docs

- [Kubernetes & Helm](/docs/kubernetes-helm/)
- [OTel Compatibility](/docs/otel-compatibility/)
- [UI Walkthrough](/docs/ui-walkthrough/)
