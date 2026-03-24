---
title: "Quick Start"
weight: 1
description: "Get up and running with AgentEvals in under 5 minutes."
---

## Installation

**From PyPI** (recommended): the published package includes the **CLI**, **REST API**, and **embedded web UI**.

```bash
pip install agentevals-cli
```

Optional extras:

```bash
pip install "agentevals-cli[live]"        # MCP server (`agentevals mcp`)
```

**GitHub [releases page](https://github.com/agentevals-dev/agentevals/releases)** also ships **core** wheels (CLI and API only) and **bundle** wheels (with the embedded UI) if you need a specific version or offline `pip install ./path/to.whl`. For MCP support, install with the `[live]` extra the same way as from PyPI (for example `pip install "./your-wheel.whl[live]"`).

**From source** with `uv` or Nix:

```bash
uv sync
# or: nix develop .
```

See [DEVELOPMENT.md](https://github.com/agentevals-dev/agentevals/blob/main/DEVELOPMENT.md) for build instructions.

## CLI Quick Start

Run an evaluation against a sample trace:

```bash
agentevals run samples/helm.json \
  --eval-set samples/eval_set_helm.json \
  -m tool_trajectory_avg_score
```

List available evaluators:

```bash
agentevals evaluator list
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

- [Integrations](/docs/integrations/) — Zero-code, SDK, and CLI/CI integration patterns
- [Custom Evaluators](/docs/custom-evaluators/) — Build your own evaluators
- [UI Walkthrough](/docs/ui-walkthrough/) — Deep dive into the web UI
