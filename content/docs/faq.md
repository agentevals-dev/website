---
title: "FAQ"
weight: 6
description: "Frequently asked questions about AgentEvals."
---

## How does this compare to ADK's evaluations?

Unlike ADK's LocalEvalService, which couples agent execution with evaluation, agentevals only handles scoring: it takes pre-recorded traces and compares them against expected behavior using metrics like tool trajectory matching, response quality, and LLM-based judgments.

However, if you're iterating on your agents locally, you can point your agents to agentevals and you will see rich runtime information in your browser. For more details, use the bundled wheel and explore the Local Development option in the UI.

## How does this compare to Bedrock AgentCore's evaluation?

AgentCore's evaluation integration (via `strands-agents-evals`) also couples agent execution with evaluation. It re-invokes the agent for each test case, converts the resulting OTel spans to AWS's ADOT format, and scores them against 4 built-in evaluators (Helpfulness, Accuracy, Harmfulness, Relevance) via a cloud API call. This means you need an AWS account, valid credentials, and network access for every evaluation.

agentevals takes a different approach: it scores pre-recorded traces locally without re-running anything. It works with standard Jaeger JSON and OTLP formats from any framework, supports open-ended metrics (tool trajectory matching, LLM-based judges, custom scorers), and ships with a CLI, web UI, and MCP server. No cloud dependency required.

## What trace formats are supported?

AgentEvals supports **OTLP** (OpenTelemetry Protocol) with `http/protobuf` and `http/json`, plus **Jaeger JSON** trace exports. Works with any OTel-instrumented framework including LangChain, Strands, Google ADK, and others.

## Do I need to re-run my agent to evaluate it?

No. Record once, score as many times as you want. AgentEvals evaluates from existing traces, so you never need to replay expensive LLM calls.

## What frameworks are supported?

Any framework that emits OpenTelemetry spans works out of the box. This includes **LangChain**, **Strands**, **Google ADK**, and any other OTel-instrumented framework. The zero-code integration requires no SDK — just point your agent's OTel exporter to agentevals.

## Can I write custom evaluators?

Yes. Evaluators can be written in Python, JavaScript, or any language that reads JSON from stdin and writes JSON to stdout. See the [Custom Evaluators](/docs/custom-evaluators/) page for details.

A Python SDK is available (`pip install agentevals-evaluator-sdk`) for convenience, but it's not required.

## Can I use this in CI/CD?

Absolutely. The CLI is designed for CI integration. Use `--output json` for machine-readable results. See the [CLI & CI/CD section](/docs/integrations/#cli--cicd) for a GitHub Actions example.

## Is there a community evaluator registry?

Yes. Browse community-contributed evaluators on the [Evaluators](/evaluators/) page, or contribute your own to the [evaluators repository](https://github.com/agentevals-dev/evaluators).

## Is AgentEvals open source?

Yes. AgentEvals is open source and available on [GitHub](https://github.com/agentevals-dev/agentevals). Contributions are welcome!
