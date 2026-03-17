---
title: "MCP Server"
weight: 5
description: "Run evaluations from Claude Code via the MCP server."
---

## Overview

AgentEvals ships an MCP (Model Context Protocol) server that lets you run evaluations directly from Claude Code conversations. Ask Claude to evaluate a trace and get results inline.

## Setup

### 1. Install AgentEvals

```bash
pip install agentevals
```

### 2. Configure Claude Code

Add the MCP server to your Claude Code configuration:

```json
{
  "mcpServers": {
    "agentevals": {
      "command": "agentevals",
      "args": ["mcp-server"],
      "env": {
        "OPENAI_API_KEY": "your-key-here"
      }
    }
  }
}
```

### 3. Restart Claude Code

After updating your configuration, restart Claude Code to connect to the MCP server.

## Available Tools

Once connected, the MCP server exposes these tools to Claude:

### `evaluate_trace`

Run an evaluation against a trace file.

**Parameters:**
- `trace_path` — Path to the trace file (JSON)
- `eval_set_path` — Path to the eval set (YAML)

**Example conversation:**

> **You:** Evaluate the trace in `./traces/latest.json` against `./evals/golden.yaml`
>
> **Claude:** *uses evaluate_trace tool*
>
> Results:
> - greeting-check: PASS (1.0)
> - resolution-check: PASS (0.92)
> - Overall: 2/2 passed (score: 0.96)

### `list_eval_sets`

List all available eval sets in the project.

### `create_eval_set`

Interactively create a new eval set from a trace. Claude will analyze the trace and suggest evaluation criteria.

### `compare_runs`

Compare evaluation results across multiple trace files to track regression.

## Example Workflow

1. Run your agent and capture a trace
2. Ask Claude to evaluate it:

```
> Evaluate my latest agent trace against the golden eval set
```

3. Claude runs the evaluation and reports results
4. If something fails, ask Claude to analyze why:

```
> Why did the resolution-check fail? Show me the relevant spans.
```

5. Create new eval sets from traces:

```
> Create an eval set from this trace for the new feature
```
