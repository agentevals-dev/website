---
title: "Custom Evaluators"
weight: 3
description: "Write your own scoring logic in Python, JavaScript, or any language."
---

Beyond the built-in metrics, you can write your own evaluators in Python, JavaScript, or any language. An evaluator is any program that reads JSON from stdin and writes a score to stdout.

> For the comprehensive guide, see [custom-evaluators.md](https://github.com/agentevals-dev/agentevals/blob/main/docs/custom-evaluators.md) in the repository.

## Scaffold an Evaluator

```bash
agentevals evaluator init my_evaluator
```

This creates a directory with boilerplate and a manifest:

```
my_evaluator/
├── my_evaluator.py     # your scoring logic
└── evaluator.yaml      # metadata manifest
```

You can also list supported runtimes and generate config snippets:

```bash
agentevals evaluator runtimes              # show supported languages
agentevals evaluator config my_evaluator \
  --path ./evaluators/my_evaluator.py      # generate config snippet
```

## Implement Scoring Logic

Your function receives an `EvalInput` with the agent's invocations and returns an `EvalResult` with a score between 0.0 and 1.0.

```python
from agentevals_evaluator_sdk import EvalInput, EvalResult, evaluator

@evaluator
def my_evaluator(input: EvalInput) -> EvalResult:
    scores = []
    for inv in input.invocations:
        # Your scoring logic here
        score = 1.0
        scores.append(score)

    return EvalResult(
        score=sum(scores) / len(scores) if scores else 0.0,
        per_invocation_scores=scores,
    )

if __name__ == "__main__":
    my_evaluator.run()
```

Install the SDK standalone with `pip install agentevals-evaluator-sdk` (no heavy dependencies).

## Reference in Eval Config

```yaml
# eval_config.yaml
evaluators:
  - name: tool_trajectory_avg_score
    type: builtin

  - name: my_evaluator
    type: code
    path: ./evaluators/my_evaluator.py
    threshold: 0.7
```

```bash
agentevals run trace.json --config eval_config.yaml --eval-set eval_set.json
```

## Community Evaluators

Community evaluators can be referenced directly from the shared [evaluators repository](https://github.com/agentevals-dev/evaluators) using `type: remote`:

```yaml
evaluators:
  - name: response_quality
    type: remote
    source: github
    ref: evaluators/response_quality/response_quality.py
    threshold: 0.7
    config:
      min_response_length: 20
```

Browse available community evaluators on the [Evaluators](/evaluators/) page, or contribute your own.

## Supported Languages

Evaluators can be written in any language that reads JSON from stdin and writes JSON to stdout.

| Language | Extension | SDK available |
|---|---|---|
| Python | `.py` | `pip install agentevals-evaluator-sdk` |
| JavaScript | `.js` | No SDK yet — just read stdin, write stdout |
| TypeScript | `.ts` | No SDK yet — just read stdin, write stdout |

## Further Reading

- [Custom Evaluators Guide](https://github.com/agentevals-dev/agentevals/blob/main/docs/custom-evaluators.md) — Full protocol reference
- [Community Evaluators](/evaluators/) — Browse and submit evaluators
- [Eval Set Format](https://github.com/agentevals-dev/agentevals/blob/main/docs/eval-set-format.md) — Schema and field reference for eval set JSON files
