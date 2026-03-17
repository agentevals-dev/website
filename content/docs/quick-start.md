---
title: "🚀 Quick Start"
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

## Your First Evaluation

### 1. Export a trace

If your agent is instrumented with OpenTelemetry, export a trace as a JSON file:

```bash
agentevals capture --otlp-port 4318 --output ./traces/my-agent.json
```

Or if you already have a Jaeger JSON export:

```bash
# Use it directly — no conversion needed
ls ./traces/jaeger-export.json
```

### 2. Create a golden eval set

Create a YAML file that describes the expected behavior:

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

### 4. Start the Web UI (optional)

```bash
agentevals ui
```

Open `http://localhost:8080` to visually inspect traces and evaluation results.

## Next Steps

- Read the [Configuration](/docs/configuration/) guide to customize evaluators
- See [Examples](/docs/examples/) for common evaluation patterns
- Set up [CI/CD Integration](/docs/ci-cd/) for automated evaluation
