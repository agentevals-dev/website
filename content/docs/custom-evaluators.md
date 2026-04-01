---
title: Custom Evaluators
weight: 3
description: Define custom evaluation logic for agentevals when built-in metrics are not enough.
---

Custom evaluators let you add project-specific scoring logic on top of the trace data agentevals extracts.

Use custom evaluators when:

- you need domain-specific scoring rules
- built-in metrics do not capture the behavior you care about
- you want deterministic checks alongside model-based judges
- you want to combine trace metadata with output inspection

## When to use custom evaluators vs delegated backends

Use **custom evaluators** when the evaluation logic should live in your own codebase.

Use a **delegated backend** such as the [OpenAI Evals API backend](/docs/openai-evals-api/) when you want agentevals to package data and send judging to an external evaluation system.

## What custom evaluators operate on

Custom evaluators work on normalized data extracted from traces. In practice, that means you can reason about:

- prompts and responses
- tool calls and tool results
- metadata attached to spans or traces
- expected outputs or dataset annotations, when present

The exact structure depends on your eval configuration and trace contents.

## General workflow

1. define the eval set and metrics you want to run
2. implement a Python evaluator for your scoring logic
3. register or reference it from your eval configuration
4. run agentevals against your trace data
5. inspect the resulting scores in CLI or UI

## Good evaluator design principles

A strong custom evaluator is usually:

- **focused** on one behavior or failure mode
- **repeatable** so results are easy to compare over time
- **well-named** so metrics are readable in reports
- **trace-aware** so it relies on durable attributes instead of brittle formatting assumptions

## Common patterns

### Deterministic checks

Examples:

- required tool was called
- forbidden tool was not called
- final answer included a required field
- workflow completed within a step limit

### Rubric-based scoring

Examples:

- answer relevance
- factual grounding against context
- adherence to response format
- success at completing a user task

### Hybrid scoring

Many teams combine deterministic checks with model-based judging. For example:

- fail if a critical tool call is missing
- otherwise apply a quality rubric score

## Related docs

- [Eval Set Format](/docs/eval-set-format/)
- [OTel Compatibility](/docs/otel-compatibility/)
- [OpenAI Evals API backend](/docs/openai-evals-api/)
- [Streaming](/docs/streaming/)

## Recommendation

Start with the smallest evaluator that captures a real product risk. Add more evaluators only when they create a clear signal you intend to track over time.
