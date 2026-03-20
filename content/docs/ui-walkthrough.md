---
title: "UI Walkthrough"
weight: 4
description: "Visual guide to the AgentEvals web interface."
---

The AgentEvals Web UI provides a visual interface for inspecting traces, running evaluations, and building eval sets.

## Starting the UI

```bash
# Start on default port
agentevals ui

# Start on a custom port
agentevals ui --port 3000

# Start with live OTLP receiver
agentevals ui --otlp-port 4318
```

## Home Screen

The home screen gives you three workflows:

- **Live Streaming** — Watch traces and evaluations in real-time as you develop
- **Offline Evaluations** — Upload traces and eval sets to run batch evaluations
- **EvalSet Builder** — Turn recorded traces into golden eval sets

## Trace Inspector

The trace inspector provides a detailed view of each trace:

- **Waterfall view** — Visualize span timing and hierarchy
- **Span details** — View attributes, events, status, and duration
- **Tool calls** — See every tool invocation with arguments and results
- **LLM calls** — Inspect prompts, completions, token usage, and latency
- **Evaluation overlay** — See which spans matched evaluation criteria

## Live Streaming Mode

Connect the UI to a running agent to see traces in real-time:

```bash
agentevals ui --otlp-port 4318
```

Point your agent's OTLP exporter to `http://localhost:4318`. Traces appear in the UI as they arrive.

**Features:**
- Real-time trace visualization with span waterfall
- Automatic evaluation as traces complete
- Live pass/fail indicators
- Span detail inspector with attributes, events, and links

## Running Offline Evaluations

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

## Diff View

Compare two agent runs side-by-side to understand behavioral changes:

- See which tool calls changed between runs
- Compare evaluation scores across versions
- Identify regressions quickly

## Exporting Results

Export evaluation results from the UI:

- **JSON** — Full results with scores, reasoning, and metadata
- **CSV** — Tabular format for spreadsheets and reporting
- **YAML** — Eval set format for reuse with the CLI
