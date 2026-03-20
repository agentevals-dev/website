---
title: "Custom Evaluators"
weight: 3
description: "Build your own evaluators for domain-specific evaluation logic."
---

Beyond the built-in evaluators, AgentEvals supports creating custom evaluators for domain-specific evaluation logic.

> For the comprehensive custom evaluators guide, see [custom-evaluators.md](https://github.com/agentevals-dev/agentevals/blob/main/docs/custom-evaluators.md) in the repository.

## Built-in Evaluator Types

AgentEvals ships with three families of evaluators:

### Trajectory Match

Deterministic comparison of agent trajectories against reference outputs. Fast, free, and reproducible.

**Matching modes:**
- **strict** — Exact match: identical messages and tool calls in the same order
- **unordered** — All required tools must be called, but order doesn't matter
- **subset** — Agent calls only the reference tools (no extras allowed)
- **superset** — Agent calls at least all reference tools (extras are OK)

**Tool argument matching:**
- **exact** — Tool arguments must match precisely (default)
- **ignore** — Disregard argument differences entirely
- **subset** / **superset** — Partial argument matching
- **Custom matchers** — Per-tool matching logic via `tool_args_match_overrides`

### LLM-as-Judge

Uses a language model to evaluate trajectory quality semantically. Supports custom prompts, few-shot examples, continuous scoring (0.0–1.0), and both with-reference and without-reference evaluation.

**Built-in prompt templates:**
- `TRAJECTORY_ACCURACY_PROMPT` — Quality-only evaluation (no reference needed)
- `TRAJECTORY_ACCURACY_PROMPT_WITH_REFERENCE` — Compares against a reference trajectory (default)
- `GRAPH_TRAJECTORY_ACCURACY_PROMPT` — Specialized for graph-based evaluation

### Graph Trajectory

Specialized for LangGraph workflows. Includes LLM-as-judge and strict match variants, with utilities for extracting trajectories from graph state snapshots and threads.

## Custom Per-Tool Argument Matching

### Python

```python
from agentevals.trajectory.match import (
    create_trajectory_match_evaluator
)

evaluator = create_trajectory_match_evaluator(
    trajectory_match_mode="superset",
    tool_args_match_mode="exact",
    tool_args_match_overrides={
        # Ignore args for search (queries vary)
        "search_flights": "ignore",
        # Only check specific keys for booking
        "book_flight": ["flight_id", "passenger_name"],
        # Custom matcher function
        "send_email": lambda actual, expected: (
            actual["to"] == expected["to"]
        ),
    }
)
```

### TypeScript

```typescript
import { createTrajectoryMatchEvaluator } from "agentevals";

const evaluator = createTrajectoryMatchEvaluator({
  trajectoryMatchMode: "superset",
  toolArgsMatchMode: "exact",
  toolArgsMatchOverrides: {
    // Ignore args for search (queries vary)
    search_flights: "ignore",
    // Only check specific keys for booking
    book_flight: ["flight_id", "passenger_name"],
    // Custom matcher function
    send_email: (actual, expected) =>
      actual.to === expected.to,
  }
});
```

## LLM-as-Judge with Custom Prompts

### Python

```python
from agentevals.trajectory.llm import (
    create_trajectory_llm_as_judge
)

evaluator = create_trajectory_llm_as_judge(
    model="openai:o3-mini",
    prompt="""Evaluate whether the agent followed the company's
    escalation policy. The agent should:
    1. Attempt to resolve the issue directly
    2. If unable, ask the customer to hold
    3. Transfer to a supervisor with context
    Score 1.0 if all steps followed, 0.0 otherwise.""",
    continuous=True,
    use_reasoning=True
)
```

### TypeScript

```typescript
import { createTrajectoryLLMAsJudge } from "agentevals";

const evaluator = createTrajectoryLLMAsJudge({
  model: "openai:o3-mini",
  prompt: `Evaluate whether the agent followed the company's
    escalation policy. The agent should:
    1. Attempt to resolve the issue directly
    2. If unable, ask the customer to hold
    3. Transfer to a supervisor with context
    Score 1.0 if all steps followed, 0.0 otherwise.`,
  continuous: true,
  useReasoning: true
});
```

## Graph Trajectory Evaluation

For LangGraph workflows, use the specialized graph trajectory evaluators:

### Python

```python
from agentevals.graph_trajectory import (
    create_graph_trajectory_llm_as_judge,
    graph_trajectory_strict_match,
    extract_langgraph_trajectory_from_thread
)

# Extract trajectory from a LangGraph thread
trajectory = extract_langgraph_trajectory_from_thread(
    thread_id="my-thread",
    graph=my_graph
)

# Evaluate with LLM judge
evaluator = create_graph_trajectory_llm_as_judge()
result = evaluator(outputs=trajectory.outputs)

# Or use strict matching
result = graph_trajectory_strict_match(
    outputs=trajectory.outputs,
    reference_outputs=expected
)
```

## Further Reading

- [Full Custom Evaluators Guide](https://github.com/agentevals-dev/agentevals/blob/main/docs/custom-evaluators.md)
- [Examples Directory](https://github.com/agentevals-dev/agentevals/tree/main/examples)
