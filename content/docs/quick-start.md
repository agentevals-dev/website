---
title: "Quick Start"
weight: 1
description: "Get started with agentevals in minutes"
---

# Quick Start

Get from zero to your first evaluation in under 5 minutes.

## 1. Install

```bash
npm install -g @agentevals/agentv
```

Verify the installation:

```bash
agentv --version
```

## 2. Create an Eval

Create a file named `EVAL.yaml`:

```yaml
suite: customer-support-evals
version: 1

cases:
  - name: tool_usage_validation
    target: support-bot
    criteria: Agent should use search_docs before answering policy questions
    evaluators:
      - type: tool_trajectory
        expected_sequence: [search_docs, format_answer]
        allow_extra_steps: true
```

## 3. Run Your Eval

Execute your evaluation suite:

```bash
agentv run --eval EVAL.yaml
```

## 4. View Results

Get detailed results in the terminal:

```bash
agentv run --eval EVAL.yaml --format table
```

Export as JSON for CI/CD or further processing:

```bash
agentv run --eval EVAL.yaml --format json > results.json
```

## 5. Try the Examples

Explore sample evaluation suites:

```bash
npx agentv run --eval examples/customer-support/EVAL.yaml
npx agentv run --eval examples/code-review/EVAL.yaml
```

## Next Steps

- **Learn the YAML format** → [Configuration](/docs/configuration/)
- **See more examples** → [Examples](/docs/examples/)
- **Set up CI/CD** → [CI/CD Integration](/docs/ci-cd/)
- **Use the MCP server** → [MCP Server](/docs/mcp-server/)
