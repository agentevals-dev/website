---
title: "Integrations & Use Cases"
weight: 2
description: "Zero-code, SDK, CLI/CI, and MCP integration patterns."
---

AgentEvals can be used in multiple ways depending on your workflow. Evaluate agents with zero code using YAML-based eval sets, programmatically via the SDK, in CI pipelines, or conversationally through the MCP server.

> For detailed, working examples covering all integration patterns, see the [examples directory](https://github.com/agentevals-dev/agentevals/tree/main/examples) in the repository. The examples are comprehensive and production-quality.

---

## Zero-Code Evaluation

Define evaluations entirely in YAML — no code required. Create an eval set file that describes the expected agent behavior, then run it against traces using the CLI.

```yaml
# evals/golden.yaml
name: booking-agent-evals
description: Golden eval set for the booking agent

evaluators:
  - name: calls-search-tool
    type: trajectory_match
    match_mode: superset
    expected_tools:
      - search_flights
      - search_hotels

  - name: confirms-before-booking
    type: llm_as_judge
    prompt: |
      Does the agent ask the user for confirmation
      before making a booking?

  - name: no-hallucinated-prices
    type: llm_as_judge
    prompt: |
      Does the agent only quote prices that were
      returned by tool calls?
```

Run it:

```bash
agentevals eval --trace ./traces/booking-run.json \
  --eval-set ./evals/golden.yaml
```

That's it. No Python, no TypeScript — just YAML and a trace file.

---

## AgentEvals SDK

For programmatic control, use the SDK directly. This gives you full flexibility to compose evaluators, define custom scoring logic, and integrate into your test suite.

### Python

```python
from agentevals.trajectory.match import (
    create_trajectory_match_evaluator
)
from agentevals.trajectory.llm import (
    create_trajectory_llm_as_judge
)

# Trajectory matching with flexible modes
match_eval = create_trajectory_match_evaluator(
    trajectory_match_mode="superset",
    tool_args_match_mode="ignore"
)

result = match_eval(
    outputs=actual_trajectory,
    reference_outputs=expected_trajectory
)
print(f"Match: {result.score}")

# LLM-as-judge for semantic evaluation
llm_eval = create_trajectory_llm_as_judge(
    model="openai:o3-mini",
    use_reasoning=True,
    continuous=True  # 0.0-1.0 score
)

result = llm_eval(
    outputs=trajectory,
    reference_outputs=reference
)
print(f"Score: {result.score}")
print(f"Reasoning: {result.comment}")
```

### TypeScript

```typescript
import {
  createTrajectoryMatchEvaluator,
  createTrajectoryLLMAsJudge
} from "agentevals";

// Trajectory matching with flexible modes
const matchEval = createTrajectoryMatchEvaluator({
  trajectoryMatchMode: "superset",
  toolArgsMatchMode: "ignore"
});

const matchResult = await matchEval({
  outputs: actualTrajectory,
  referenceOutputs: expectedTrajectory
});
console.log(`Match: ${matchResult.score}`);

// LLM-as-judge for semantic evaluation
const llmEval = createTrajectoryLLMAsJudge({
  model: "openai:o3-mini",
  useReasoning: true,
  continuous: true
});

const llmResult = await llmEval({
  outputs: trajectory,
  referenceOutputs: reference
});
console.log(`Score: ${llmResult.score}`);
```

For more SDK examples including graph trajectory evaluation, async usage, custom prompts, and few-shot examples, see the [examples directory](https://github.com/agentevals-dev/agentevals/tree/main/examples).

---

## CLI & CI/CD

AgentEvals is designed to run in CI pipelines. Gate deployments on agent behavior quality scores.

### CLI Commands

```bash
# Evaluate a trace against an eval set
agentevals eval --trace ./traces/run.json \
  --eval-set ./evals/golden.yaml

# Output as JSON for scripting
agentevals eval --trace ./traces/run.json \
  --eval-set ./evals/golden.yaml --output json

# Output as JUnit XML for CI test reporters
agentevals eval --trace ./traces/run.json \
  --eval-set ./evals/golden.yaml \
  --output junit --output-file results.xml

# Set a minimum score threshold (exits 1 if below)
agentevals eval --trace ./traces/run.json \
  --eval-set ./evals/golden.yaml \
  --min-score 0.85 --fail-on-below
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

      - name: Install dependencies
        run: |
          pip install agentevals
          pip install -r requirements.txt

      - name: Run agent and capture trace
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          python run_agent.py --capture-trace ./traces/pr-run.json

      - name: Evaluate agent behavior
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          agentevals eval \
            --trace ./traces/pr-run.json \
            --eval-set ./evals/golden.yaml \
            --output junit \
            --output-file ./results/eval-results.xml

      - name: Publish results
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Agent Evaluation Results
          path: ./results/eval-results.xml
          reporter: java-junit
```

### Exit Codes

| Code | Meaning |
|------|---------|
| `0` | All evaluations passed |
| `1` | One or more evaluations failed |
| `2` | Configuration or runtime error |

---

## MCP Server

Run evaluations directly from Claude Code (or any MCP-compatible client). The MCP server exposes AgentEvals as tools that AI agents can invoke.

### Setup

```bash
# Start the MCP server
agentevals mcp-server
```

Add to your Claude Code configuration:

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

### Available MCP Tools

- **`evaluate_trace`** — Run an evaluation against a trace file
- **`list_eval_sets`** — List all available eval sets in the project
- **`create_eval_set`** — Interactively create a new eval set from a trace
- **`compare_runs`** — Compare evaluation results across multiple traces

### Example Conversation

> **You:** Evaluate the trace in `./traces/latest.json` against `./evals/golden.yaml`
>
> **Claude:** *uses evaluate_trace tool*
>
> Results:
> - greeting-check: PASS (1.0)
> - resolution-check: PASS (0.92)
> - Overall: 2/2 passed (score: 0.96)

> **You:** Why did the resolution-check fail? Show me the relevant spans.

This lets you iterate on evaluations conversationally without switching between tools.
