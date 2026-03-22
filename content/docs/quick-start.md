---
title: "Quick Start"
weight: 1
description: "Get up and running with AgentEvals in under 5 minutes."
---

## Installation

Grab a wheel from the [releases page](https://github.com/agentevals-dev/agentevals/releases). The **core** wheel has the CLI and REST API. The **bundle** wheel adds streaming and the embedded web UI.

```bash
pip install agentevals-<version>-py3-none-any.whl

# For MCP server and live streaming support:
pip install "agentevals-<version>-py3-none-any.whl[live]"
```

**From source** with `uv` or Nix:

```bash
uv sync
# or: nix develop .
```

See [DEVELOPMENT.md](https://github.com/agentevals-dev/agentevals/blob/main/DEVELOPMENT.md) for build instructions.

## CLI Quick Start

Run an evaluation against a sample trace:

```bash
uv run agentevals run samples/helm.json \
  --eval-set samples/eval_set_helm.json \
  -m tool_trajectory_avg_score
```

List available evaluators:

```bash
uv run agentevals evaluator list
```

## Live UI Quick Start

Start the server with the embedded web UI:

```bash
agentevals serve
```

Open `http://localhost:8001` to upload traces and eval sets, select metrics, and view results with interactive span trees.

**From source** (two terminals):

```bash
uv run agentevals serve --dev     # Terminal 1
cd ui && npm install && npm run dev  # Terminal 2 → http://localhost:5173
```

Live-streamed traces appear in the "Local Dev" tab, grouped by session ID.

## What's Next

- [Integrations](/docs/integrations/) — Zero-code, SDK, CLI/CI, and MCP integration patterns
- [Custom Evaluators](/docs/custom-evaluators/) — Build your own evaluators
- [UI Walkthrough](/docs/ui-walkthrough/) — Deep dive into the web UI
