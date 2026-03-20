---
title: "UI Walkthrough"
weight: 4
description: "Visual guide to the AgentEvals web interface."
---

The AgentEvals Web UI provides a visual interface for uploading traces, running evaluations, and inspecting results with interactive span trees.

## Starting the UI

**Installed bundle** (port 8001):

```bash
agentevals serve
```

**From source** (two terminals):

```bash
uv run agentevals serve --dev      # Terminal 1
cd ui && npm install && npm run dev   # Terminal 2 → http://localhost:5173
```

## Key Features

### Trace Upload & Evaluation

Upload traces and eval sets, select metrics, and view results. Supports both **Jaeger JSON** and **OTLP** trace formats.

### Interactive Span Trees

Drill into agent execution with interactive span trees showing:
- Span timing and hierarchy
- Tool calls with arguments and results
- LLM calls with prompts, completions, and token usage
- Evaluation overlay showing which spans matched criteria

### Live Streaming

Live-streamed traces appear in the **Local Dev** tab, grouped by session ID. Connect any OTel-instrumented agent:

```bash
# Start the server with dev mode
uv run agentevals serve --dev

# Point your agent's OTel exporter to agentevals
export OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318
export OTEL_RESOURCE_ATTRIBUTES="agentevals.session_name=my-agent"
python your_agent.py
```

Traces stream in real-time. Works with LangChain, Strands, Google ADK, or any OTel-compatible framework.

### Session Management

Sessions are auto-created and grouped by `agentevals.session_name`. Set `agentevals.eval_set_id` to associate traces with an eval set for automatic evaluation.

## REST API

While the server is running, interactive API documentation is available:

| Endpoint | Description |
|----------|-------------|
| [`/docs`](http://localhost:8001/docs) | Swagger UI with interactive request builder |
| [`/redoc`](http://localhost:8001/redoc) | ReDoc reference documentation |
| [`/openapi.json`](http://localhost:8001/openapi.json) | Raw OpenAPI 3.x schema (for code generation or CI) |

The OTLP receiver (port 4318) serves its own docs at `http://localhost:4318/docs`.
