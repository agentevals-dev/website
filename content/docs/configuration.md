---
title: "Configuration"
weight: 2
description: "Configure evaluators, trace sources, and output formats."
---

## Configuration File

AgentEvals uses a `agentevals.yaml` configuration file in your project root:

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

## Trace Sources

### OTLP (OpenTelemetry Protocol)

Receive traces directly from your instrumented agent:

```yaml
trace_sources:
  - type: otlp
    port: 4318          # HTTP port (default: 4318)
    grpc_port: 4317     # gRPC port (default: 4317)
    protocol: http      # http or grpc
```

### Jaeger JSON

Load traces from exported Jaeger JSON files:

```yaml
trace_sources:
  - type: jaeger
    path: ./traces/export.json    # single file
    # or
    path: ./traces/*.json         # glob pattern
```

### File-based OTLP

Load traces from OTLP JSON files exported by collectors:

```yaml
trace_sources:
  - type: otlp_file
    path: ./traces/otlp-export.json
```

## Evaluator Configuration

### Trajectory Match

```yaml
evaluations:
  - name: tool-usage-check
    criteria:
      - type: trajectory_match
        mode: strict          # strict, unordered, subset, superset
        args_match: exact     # exact, ignore, subset, superset
        expected_tools:
          - name: search_database
            args:
              query: "customer issue"
          - name: create_ticket
```

### LLM-as-Judge

```yaml
evaluations:
  - name: quality-check
    criteria:
      - type: llm_judge
        model: gpt-4o          # override default model
        scoring: continuous    # binary or continuous (0.0 - 1.0)
        prompt: |
          Evaluate the agent's response quality.
          Consider helpfulness, accuracy, and completeness.
```

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `AGENTEVALS_CONFIG` | Path to config file | `./agentevals.yaml` |
| `OPENAI_API_KEY` | OpenAI API key for LLM judge | — |
| `ANTHROPIC_API_KEY` | Anthropic API key for LLM judge | — |
| `AGENTEVALS_LOG_LEVEL` | Log level (debug, info, warn, error) | `info` |
| `AGENTEVALS_OUTPUT_FORMAT` | Output format override | `table` |
