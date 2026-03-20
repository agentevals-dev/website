---
title: "FAQ"
weight: 6
description: "Frequently asked questions about AgentEvals."
---

## What trace formats are supported?

AgentEvals supports OTLP (OpenTelemetry Protocol) streams and Jaeger JSON trace exports. You can pipe traces from any OpenTelemetry-compatible observability platform, or export Jaeger JSON files directly.

## Do I need to re-run my agent to evaluate it?

No. AgentEvals evaluates from existing traces, so you never need to replay expensive LLM calls. Collect traces once, evaluate many times with different eval sets.

## What languages are supported?

AgentEvals has first-class support for both Python and TypeScript/JavaScript. The SDK APIs are identical across both languages, and async is fully supported in both.

## What is the difference between trajectory match and LLM-as-judge?

**Trajectory match** is deterministic — it compares tool calls and arguments using exact rules (strict, unordered, subset, superset). It's fast, reproducible, and free.

**LLM-as-judge** uses a language model to semantically evaluate trajectory quality. It can assess nuanced behavior (e.g., "did the agent ask for confirmation before booking?") but requires an LLM API call and incurs cost.

Use trajectory match for structural checks, and LLM-as-judge for behavioral/semantic checks. They work great together.

## Can I use AgentEvals with LangGraph?

Yes. AgentEvals includes specialized graph trajectory evaluators and utilities for extracting trajectories directly from LangGraph threads and state snapshots. See the [Custom Evaluators](/docs/custom-evaluators/) page for details.

## Can I use this in CI/CD?

Absolutely. The CLI is designed for CI integration. Use `--output json` or `--output junit` for machine-readable results and `--min-score` with `--fail-on-below` to fail the pipeline when evaluations don't pass. See the [Integrations](/docs/integrations/) page for a GitHub Actions example.

## What LLM providers are supported for LLM-as-judge?

AgentEvals supports OpenAI and Anthropic models out of the box via their respective API keys. You can also pass a custom LangChain `BaseChatModel` instance for any other provider.

## Is AgentEvals open source?

Yes. AgentEvals is open source and available on [GitHub](https://github.com/agentevals-dev/agentevals). Contributions are welcome!

## How is this different from other evaluation frameworks?

Most evaluation frameworks assess only the final output. AgentEvals focuses on evaluating the *trajectory* — the intermediate steps an agent takes while solving problems. This makes it easier to understand how changes affect system behavior, catch regressions in tool usage patterns, and ensure agents follow expected workflows.
