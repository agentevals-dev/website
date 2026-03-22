---
title: "Integrations & Use Cases"
weight: 2
description: "Zero-code, SDK, and CLI/CI integration patterns."
---

AgentEvals can be used in multiple ways depending on your workflow. Evaluate agents with zero code via OTel, programmatically via the SDK, or in CI pipelines with the CLI.

> For detailed, working examples covering all integration patterns, see the [examples directory](https://github.com/agentevals-dev/agentevals/tree/main/examples) in the repository.

---

## Zero-Code (Recommended)

Point any OTel-instrumented agent at the receiver. No SDK, no code changes:

```bash
# Terminal 1 — start the agentevals server
uv run agentevals serve --dev

# Terminal 2 — run your agent with OTel pointing to agentevals
export OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318
export OTEL_RESOURCE_ATTRIBUTES="agentevals.session_name=my-agent"
python your_agent.py
```

Traces stream to the UI in real-time. Works with **LangChain**, **Strands**, **Google ADK**, or any framework that emits OTel spans (`http/protobuf` and `http/json` supported).

Sessions are auto-created and grouped by `agentevals.session_name`. Set `agentevals.eval_set_id` to associate traces with an eval set.

See [examples/zero-code-examples/](https://github.com/agentevals-dev/agentevals/tree/main/examples/zero-code-examples) for working examples with different frameworks.

---

## AgentEvals SDK

For programmatic session lifecycle and decorator API:

```python
from agentevals import AgentEvals

app = AgentEvals()

with app.session(eval_set_id="my-eval"):
    agent.invoke("Roll a 20-sided die for me")
```

Requires `pip install "agentevals[streaming]"`. See [examples/sdk_example/](https://github.com/agentevals-dev/agentevals/tree/main/examples/sdk_example) for framework-specific patterns.

---

## CLI & CI/CD

The CLI is built for scripting and CI pipelines.

### Commands

```bash
# Single trace
uv run agentevals run samples/helm.json \
  --eval-set samples/eval_set_helm.json \
  -m tool_trajectory_avg_score

# Multiple traces
uv run agentevals run samples/helm.json samples/k8s.json \
  --eval-set samples/eval_set_helm.json \
  -m tool_trajectory_avg_score

# JSON output for programmatic processing
uv run agentevals run samples/helm.json \
  --eval-set samples/eval_set_helm.json \
  --output json

# List available evaluators (builtin + community)
uv run agentevals evaluator list

# List only builtin evaluators
uv run agentevals evaluator list --source builtin
```

### GitHub Actions Example

```yaml
# .github/workflows/agent-eval.yml
name: Agent Evaluation

on:
  pull_request:
    paths:
      - 'agent/**'
      - 'evals/**'

jobs:
  evaluate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install agentevals
        run: pip install agentevals-*.whl

      - name: Run agent and capture trace
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: python run_agent.py --capture-trace ./traces/pr-run.json

      - name: Evaluate agent behavior
        run: |
          agentevals run ./traces/pr-run.json \
            --eval-set ./evals/golden.json \
            -m tool_trajectory_avg_score \
            --output json > results.json

      - name: Check results
        run: |
          python -c "
          import json, sys
          results = json.load(open('results.json'))
          if results['overall_score'] < 0.85:
              print(f'Score {results[\"overall_score\"]} below threshold')
              sys.exit(1)
          "
```

