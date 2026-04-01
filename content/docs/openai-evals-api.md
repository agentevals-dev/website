---
title: OpenAI Evals API Backend
weight: 11
description: Delegate evaluation to OpenAI's Evals API while keeping agentevals as the trace-oriented evaluation layer.
---

agentevals includes an initial option to delegate evals to OpenAI's Evals API.

## What this means

Instead of keeping all judging logic inside agentevals, you can use agentevals to:

- extract and normalize data from traces
- organize evaluation inputs
- send evaluation work to OpenAI's Evals API backend
- collect and review the resulting outputs in your agentevals workflow

## When to use it

This backend is a good fit when:

- you want external rubric-based or model-based judging
- you already use OpenAI tooling in your evaluation stack
- you want agentevals to stay focused on trace extraction and orchestration

## When not to use it

Prefer [Custom Evaluators](/docs/custom-evaluators/) when:

- the logic should remain fully local and code-defined
- you need deterministic checks with no external dependency
- you want complete control over evaluator implementation details

## Requirements

To use this backend, you should expect to provide:

- an OpenAI API key
- an eval definition compatible with your delegated workflow
- trace data that contains enough context for judging

## Conceptual workflow

1. collect traces from agent runs
2. map them into the eval structure agentevals expects
3. configure the OpenAI backend
4. delegate evaluation to OpenAI's Evals API
5. review results through agentevals outputs and UI

## Recommendation

Start with one narrow delegated eval first, confirm the judgment shape matches your expectations, then expand to broader use cases.

## Related docs

- [Quick Start](/docs/quick-start/)
- [Advanced](/docs/advanced/)
- [Custom Evaluators](/docs/custom-evaluators/)
- [Eval Set Format](/docs/eval-set-format/)
- [Integrations](/docs/integrations/)
