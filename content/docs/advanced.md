---
title: "Advanced"
weight: 5
description: "Deep-dive documentation, REST API, and development setup."
---

## Docs

| Guide | Description |
|-------|-------------|
| [Eval Set Format](https://github.com/agentevals-dev/agentevals/blob/main/docs/eval-set-format.md) | Schema, field reference, and examples for golden eval set JSON files |
| [Custom Evaluators](https://github.com/agentevals-dev/agentevals/blob/main/docs/custom-evaluators.md) | Write your own scoring logic in Python, JavaScript, or any language |
| [Live Streaming](https://github.com/agentevals-dev/agentevals/blob/main/docs/streaming.md) | Real-time trace streaming, dev server setup, and session management |
| [OpenTelemetry Compatibility](https://github.com/agentevals-dev/agentevals/blob/main/docs/otel-compatibility.md) | Supported OTel conventions, message delivery mechanisms, and OTLP receiver |

## REST API Reference

While the server is running (`agentevals serve`), interactive API documentation is available at:

| Endpoint | Description |
|----------|-------------|
| [`/docs`](http://localhost:8001/docs) | Swagger UI with interactive request builder |
| [`/redoc`](http://localhost:8001/redoc) | ReDoc reference documentation |
| [`/openapi.json`](http://localhost:8001/openapi.json) | Raw OpenAPI 3.x schema (for code generation or CI) |

The OTLP receiver (port 4318) serves its own docs at `http://localhost:4318/docs`.

## Development

```bash
uv run pytest                      # run tests
uv run agentevals serve --dev      # backend
cd ui && npm run dev               # frontend (separate terminal)
```

See [DEVELOPMENT.md](https://github.com/agentevals-dev/agentevals/blob/main/DEVELOPMENT.md) for build tiers, Makefile targets, and Nix setup. To contribute, see [CONTRIBUTING.md](https://github.com/agentevals-dev/agentevals/blob/main/CONTRIBUTING.md).
