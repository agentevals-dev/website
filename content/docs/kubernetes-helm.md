---
title: Kubernetes & Helm
weight: 9
description: Deploy agentevals with Docker images and the Helm chart on Kubernetes.
---

agentevals now supports containerized deployment and Kubernetes installation through the project Helm chart.

## What this unlocks

Running agentevals in containers or Kubernetes makes it easier to:

- standardize environments across teams
- run eval workloads in CI or shared infrastructure
- expose a shared UI or evaluation service
- manage configuration in a repeatable way

## Docker image support

The project includes a Dockerfile, which means you can build and run agentevals in a containerized environment.

Typical uses include:

- local reproducible testing
- CI jobs
- scheduled evaluation workloads
- base images for Kubernetes deployment

## Helm chart support

The repository includes a Helm chart at `charts/agentevals` with chart metadata, templates, and a `values.yaml` file for configuration.

Typical Helm workflow:

```bash
helm install agentevals ./charts/agentevals
```

In practice, most teams will customize values before installing.

## What to configure

The exact settings depend on your environment, but common areas include:

- image repository and tag
- service configuration
- ingress settings
- environment variables
- secrets for model or backend providers
- persistence and resource sizing

## Recommended rollout approach

1. validate the container locally
2. review the Helm chart defaults
3. create an environment-specific values file
4. deploy to a non-production namespace first
5. verify UI and evaluation paths
6. promote after validation

## Rollback strategy

For Kubernetes deployments, prefer staged rollouts and keep a known-good values file so you can quickly roll back to the prior release.

## Related docs

- [Advanced](/docs/advanced/)
- [Integrations](/docs/integrations/)
- [UI Walkthrough](/docs/ui-walkthrough/)
- [Streaming](/docs/streaming/)
