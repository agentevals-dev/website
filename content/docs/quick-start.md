---
title: "Quick Start"
weight: 1
description: "Get up and running with AgentEvals in under 5 minutes."
---

## Prerequisites

- Python 3.10+ or Node.js 18+
- An OpenTelemetry-instrumented agent (or Jaeger JSON trace exports)
- An API key for your LLM provider (if using LLM-as-Judge evaluations)

## Installation

### Python

```bash
pip install agentevals
```

### Node.js

```bash
npm install agentevals
```

## CLI Quick Start

The fastest way to use AgentEvals is from the command line.

### 1. Export a trace

If your agent is instrumented with OpenTelemetry, capture a trace:

```bash
agentevals capture --otlp-port 4318 --output ./traces/my-agent.json
```

Or use an existing Jaeger JSON export directly — no conversion needed.

### 2. Create a golden eval set

Create a YAML file that describes expected agent behavior:

```yaml
# evals/my-eval-set.yaml
name: customer-support-agent
description: Evaluate customer support agent behavior

evaluations:
  - name: greeting-check
    description: Agent should greet the user
    criteria:
      - type: trajectory_match
        mode: subset
        expected_tools:
          - name: lookup_customer
            args_match: subset

  - name: resolution-check
    description: Agent should resolve the ticket
    criteria:
      - type: llm_judge
        prompt: |
          Did the agent successfully resolve the customer's issue?
          Consider whether the agent identified the problem, took
          appropriate actions, and confirmed resolution.
```

### 3. Run the evaluation

```bash
agentevals eval \
  --trace ./traces/my-agent.json \
  --eval-set ./evals/my-eval-set.yaml
```

Output:

```
AgentEvals v0.1.0
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Eval Set: customer-support-agent
Trace:    ./traces/my-agent.json (12 spans)

Results:
  ✓ greeting-check         PASS  (1.0)
  ✓ resolution-check       PASS  (0.92)

Overall: 2/2 passed (score: 0.96)
```

## Live UI Quick Start

Start the web UI to visually inspect traces and evaluate agent behavior:

```bash
agentevals ui
```

Open `http://localhost:8080` to:

- **Upload a trace** — Load a Jaeger JSON or OTLP JSON file
- **Browse the trajectory** — See every tool call, message, and timing in a waterfall view
- **Run evaluations** — Select an eval set and see pass/fail results with detailed reasoning
- **Build eval sets** — Turn recorded traces into golden eval sets interactively

### Live Streaming

Connect the UI to a running agent to watch traces in real-time:

```bash
agentevals ui --otlp-port 4318
```

Point your agent's OTLP exporter to `http://localhost:4318` and traces appear as they arrive.

## Next Steps

- [Integrations](/docs/integrations/) — Zero-code, SDK, CLI/CI, and MCP integration patterns
- [Custom Evaluators](/docs/custom-evaluators/) — Build your own evaluators
- [UI Walkthrough](/docs/ui-walkthrough/) — Deep dive into the web UI
