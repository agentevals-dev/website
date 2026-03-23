---
title: Quick Start
weight: 1
description: Install agentevals, point it at your traces, and run your first evaluation.
---

agentevals scores AI agent behavior from existing OpenTelemetry traces — no re-runs required.

## Install the CLI

```bash
pip install agentevals-cli
```

## Run your first evaluation

Point the CLI at an OTLP endpoint and evaluate the traces it finds.

```bash
agentevals run \
  --otlp-endpoint http://localhost:6006/v1/traces \
  --model openai/gpt-4o-mini
```

If your collector requires auth, add headers:

```bash
agentevals run \
  --otlp-endpoint https://collector.example.com/v1/traces \
  --otlp-header "Authorization=Bearer <token>" \
  --model openai/gpt-4o-mini
```

## What happens under the hood

agentevals reconstructs each traced agent interaction, sends the relevant context to an evaluator model, and writes back structured scores you can inspect in the UI or export in CI.

## Next steps

- Learn how traces, models, and outputs are configured in [Advanced](/docs/advanced/)
- Add your own scoring logic in [Custom Evaluators](/docs/custom-evaluators/)
- View and compare runs in the [UI Walkthrough](/docs/ui-walkthrough/)
