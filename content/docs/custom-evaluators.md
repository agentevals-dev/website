---
title: Custom Evaluators
weight: 3
description: Extend agentevals with your own evaluator logic in Python.
---

If the built-in rubric does not cover your use case, you can write custom evaluators in Python and run them alongside the default scoring pipeline.

## Install the evaluator SDK

```bash
pip install agentevals-evaluator-sdk
```

## Example

Create a file such as `my_eval.py`:

```python
from agentevals_evaluator import Evaluator, Score


class PolitenessEvaluator(Evaluator):
    name = "politeness"
    description = "Checks whether the agent response is polite and professional"

    def evaluate(self, trace) -> Score:
        text = trace.output_text.lower()
        passed = "please" in text or "thank you" in text
        return Score(
            value=1.0 if passed else 0.0,
            reasoning="Response includes polite phrasing" if passed else "Response is missing polite phrasing",
        )
```

Then run it with the CLI:

```bash
agentevals run \
  --otlp-endpoint http://localhost:6006/v1/traces \
  --evaluator my_eval.py:PolitenessEvaluator
```

## Tips

- Keep evaluators deterministic when possible
- Return short, useful reasoning strings for debugging
- Start with a binary pass/fail score before adding more complex grading
