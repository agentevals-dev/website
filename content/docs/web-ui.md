---
title: "Web UI"
weight: 6
description: "Visually inspect traces and evaluate agent behavior interactively."
---

## Overview

The AgentEvals Web UI provides a visual interface for inspecting traces, running evaluations, and building eval sets. It supports three primary workflows:

- **Live streaming** — Watch traces and evaluations in real-time as you develop
- **Offline evaluations** — Upload traces and eval sets to run batch evaluations
- **EvalSet Builder** — Turn recorded traces into golden eval sets

## Starting the Web UI

```bash
agentevals ui
```

By default, the UI runs on `http://localhost:8080`. Configure the port with:

```bash
agentevals ui --port 3000
```

## Live Streaming Mode

Connect the UI to a running agent to see traces in real-time:

```bash
# Start the UI with OTLP receiver enabled
agentevals ui --otlp-port 4318
```

Then point your agent's OTLP exporter to `http://localhost:4318`. Traces appear in the UI as they arrive.

### Features

- Real-time trace visualization with span waterfall
- Automatic evaluation as traces complete
- Live pass/fail indicators
- Span detail inspector with attributes, events, and links

## Offline Evaluations

Upload trace files and eval sets through the UI for batch evaluation:

1. Click **Offline Evaluations** on the home screen
2. Upload or select a trace file (OTLP JSON or Jaeger JSON)
3. Select an eval set
4. Click **Run Evaluation**
5. View detailed results with per-criterion scores

## EvalSet Builder

Create golden eval sets from recorded traces:

1. Click **EvalSet Builder** on the home screen
2. Select a trace to base the eval set on
3. The builder analyzes the trace and suggests evaluation criteria
4. Review and customize each criterion:
   - Adjust trajectory match modes
   - Edit LLM judge prompts
   - Set scoring thresholds
5. Export as YAML for use with the CLI or CI/CD

## Trace Inspector

The trace inspector provides a detailed view of each trace:

- **Waterfall view** — Visualize span timing and hierarchy
- **Span details** — View attributes, events, status, and duration
- **Tool calls** — See every tool invocation with arguments and results
- **LLM calls** — Inspect prompts, completions, token usage, and latency
- **Evaluation overlay** — See which spans matched evaluation criteria
