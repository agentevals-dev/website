---
title: "Advanced"
weight: 5
description: "Advanced configuration, API reference, and deep-dive resources."
---

## Configuration Reference

### Eval Set Configuration

```yaml
# agentevals.yaml
version: "1"

trace_sources:
  - type: otlp
    port: 4318
    protocol: http

  - type: jaeger
    path: ./traces/*.json

llm:
  provider: openai
  model: gpt-4o
  temperature: 0.0

output:
  format: table    # table, json, junit
  verbose: false
```

### Evaluator Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `trajectory_match_mode` | `"strict"` \| `"unordered"` \| `"subset"` \| `"superset"` | `"strict"` | How to compare trajectories |
| `tool_args_match_mode` | `"exact"` \| `"ignore"` \| `"subset"` \| `"superset"` | `"exact"` | How to match tool arguments |
| `tool_args_match_overrides` | `Dict[str, ...]` | `None` | Custom matchers per tool |
| `model` | `str` | `None` | LLM model for judge evaluators |
| `continuous` | `bool` | `False` | Float (0–1) vs boolean scoring |
| `use_reasoning` | `bool` | `True` | Include reasoning in output |
| `few_shot_examples` | `List[FewShotExample]` | `None` | Example evaluations for LLM judge |
| `feedback_key` | `str` | `"trajectory_accuracy"` | Key name for evaluation results |

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `AGENTEVALS_CONFIG` | Path to config file | `./agentevals.yaml` |
| `OPENAI_API_KEY` | OpenAI API key for LLM judge | — |
| `ANTHROPIC_API_KEY` | Anthropic API key for LLM judge | — |
| `AGENTEVALS_LOG_LEVEL` | Log level (debug, info, warn, error) | `info` |
| `AGENTEVALS_OUTPUT_FORMAT` | Output format override | `table` |

## Deep-Dive Documentation

For comprehensive coverage of specific topics, see the repository docs:

- [Trajectory Match Evaluators](https://github.com/agentevals-dev/agentevals#trajectory-match-evaluators) — Full reference for all matching modes and configuration
- [LLM-as-Judge Evaluators](https://github.com/agentevals-dev/agentevals#llm-as-judge-evaluators) — Custom prompts, few-shot examples, continuous scoring
- [Graph Trajectory Evaluators](https://github.com/agentevals-dev/agentevals#graph-trajectory-evaluators) — LangGraph integration and trajectory extraction
- [LangSmith Integration](https://github.com/agentevals-dev/agentevals#langsmith-integration) — Running evaluations with pytest, Vitest, and LangSmith experiments
- [Custom Evaluators Guide](https://github.com/agentevals-dev/agentevals/blob/main/docs/custom-evaluators.md) — Writing domain-specific evaluators

## Async Support

Both Python and TypeScript support fully async evaluators:

### Python

```python
from agentevals.trajectory.match import (
    create_async_trajectory_match_evaluator
)
from agentevals.trajectory.llm import (
    create_async_trajectory_llm_as_judge
)

# Async trajectory match
async_match = create_async_trajectory_match_evaluator(
    trajectory_match_mode="strict"
)
result = await async_match(
    outputs=trajectory,
    reference_outputs=reference
)

# Async LLM-as-judge
async_judge = create_async_trajectory_llm_as_judge(
    model="openai:o3-mini"
)
result = await async_judge(
    outputs=trajectory,
    reference_outputs=reference
)
```

## API Reference

### Python Public API

| Function | Description |
|----------|-------------|
| `create_trajectory_match_evaluator()` | Create a configurable trajectory match evaluator |
| `create_async_trajectory_match_evaluator()` | Async variant |
| `create_trajectory_llm_as_judge()` | Evaluate trajectory quality with an LLM judge |
| `create_async_trajectory_llm_as_judge()` | Async variant |
| `create_graph_trajectory_llm_as_judge()` | LLM-as-judge for LangGraph workflows |
| `create_async_graph_trajectory_llm_as_judge()` | Async variant |
| `graph_trajectory_strict_match()` | Strict match for graph execution steps |
| `extract_langgraph_trajectory_from_thread()` | Extract trajectory from a LangGraph thread |
| `extract_langgraph_trajectory_from_snapshots()` | Extract trajectory from state snapshots |

### TypeScript Public API

| Function | Description |
|----------|-------------|
| `createTrajectoryMatchEvaluator()` | Create a configurable trajectory match evaluator |
| `createTrajectoryLLMAsJudge()` | Evaluate trajectory quality with an LLM judge |
| `createGraphTrajectoryLLMAsJudge()` | LLM-as-judge for LangGraph workflows |
| `extractLangGraphTrajectoryFromThread()` | Extract trajectory from a LangGraph thread |
| `extractLangGraphTrajectoryFromSnapshots()` | Extract trajectory from state snapshots |
