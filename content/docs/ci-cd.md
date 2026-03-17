---
title: "CI/CD Integration"
weight: 4
description: "Run agent evaluations in your CI/CD pipeline."
---

## Overview

AgentEvals integrates into your CI/CD pipeline to gate deployments on agent behavior quality. Run evaluations on every PR or before every release.

## GitHub Actions

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

## GitLab CI

```yaml
# .gitlab-ci.yml
agent-eval:
  stage: test
  image: python:3.12
  script:
    - pip install agentevals
    - agentevals eval
        --trace ./traces/latest.json
        --eval-set ./evals/golden.yaml
        --output junit
        --output-file ./results.xml
  artifacts:
    reports:
      junit: ./results.xml
    when: always
```

## Output Formats

### JUnit XML

Use `--output junit` for CI systems that support JUnit test reports:

```bash
agentevals eval \
  --trace ./traces/run.json \
  --eval-set ./evals/golden.yaml \
  --output junit \
  --output-file results.xml
```

### JSON

Use `--output json` for programmatic processing:

```bash
agentevals eval \
  --trace ./traces/run.json \
  --eval-set ./evals/golden.yaml \
  --output json | jq '.overall_score'
```

### Exit Codes

| Code | Meaning |
|------|---------|
| `0` | All evaluations passed |
| `1` | One or more evaluations failed |
| `2` | Configuration or runtime error |

## Threshold Gates

Set minimum score thresholds to gate deployments:

```bash
agentevals eval \
  --trace ./traces/run.json \
  --eval-set ./evals/golden.yaml \
  --min-score 0.85 \
  --fail-on-below
```

The process exits with code `1` if the overall score is below the threshold.
