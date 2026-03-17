---
title: "💡 Examples"
weight: 3
description: "Common evaluation patterns and real-world usage examples."
---

## Example: Customer Support Agent

Evaluate a customer support agent that looks up customer info, checks order status, and resolves issues.

### Eval Set

```yaml
name: customer-support
description: Evaluate customer support agent behavior

evaluations:
  - name: identifies-customer
    description: Agent should look up the customer
    criteria:
      - type: trajectory_match
        mode: subset
        expected_tools:
          - name: lookup_customer
            args_match: subset
            args:
              email: "jane@example.com"

  - name: checks-order
    description: Agent should check the order status
    criteria:
      - type: trajectory_match
        mode: subset
        expected_tools:
          - name: get_order_status

  - name: resolution-quality
    description: Agent provides a helpful resolution
    criteria:
      - type: llm_judge
        scoring: continuous
        prompt: |
          Did the agent resolve the customer's issue effectively?
          Score based on:
          - Correctly identified the problem
          - Took appropriate actions
          - Communicated clearly with the customer
          - Confirmed resolution
```

### Run

```bash
agentevals eval \
  --trace ./traces/support-agent.json \
  --eval-set ./evals/customer-support.yaml
```

---

## Example: RAG Agent

Evaluate a retrieval-augmented generation agent that searches documents and synthesizes answers.

### Eval Set

```yaml
name: rag-agent
description: Evaluate RAG agent retrieval and synthesis

evaluations:
  - name: retrieves-relevant-docs
    description: Agent should search for relevant documents
    criteria:
      - type: trajectory_match
        mode: subset
        expected_tools:
          - name: vector_search
          - name: rerank_results

  - name: answer-quality
    description: Synthesized answer should be accurate
    criteria:
      - type: llm_judge
        scoring: continuous
        prompt: |
          Evaluate the agent's answer based on the retrieved documents.
          Did it accurately synthesize information?
          Did it cite sources appropriately?
          Is the answer complete and not hallucinated?

  - name: no-hallucination
    description: Agent should not hallucinate facts
    criteria:
      - type: llm_judge
        scoring: binary
        prompt: |
          Does the agent's final response contain any information
          that is NOT supported by the retrieved documents?
          Answer PASS if all claims are supported, FAIL otherwise.
```

---

## Example: Code Generation Agent

Evaluate an agent that writes and executes code.

### Eval Set

```yaml
name: code-gen-agent
description: Evaluate code generation agent

evaluations:
  - name: writes-code
    description: Agent should generate code
    criteria:
      - type: trajectory_match
        mode: subset
        expected_tools:
          - name: write_file
          - name: execute_code

  - name: runs-tests
    description: Agent should validate with tests
    criteria:
      - type: trajectory_match
        mode: subset
        expected_tools:
          - name: run_tests

  - name: code-quality
    description: Generated code should be correct
    criteria:
      - type: llm_judge
        scoring: continuous
        prompt: |
          Evaluate the generated code for:
          - Correctness: does it solve the stated problem?
          - Style: is it clean and idiomatic?
          - Safety: are there any security issues?
```

---

## Example: Using the Python SDK

```python
from agentevals import evaluate, TrajectoryMatch, LLMJudge

# Load a trace
trace = load_trace("./traces/my-agent.json")

# Define evaluators programmatically
results = evaluate(
    trace=trace,
    evaluators=[
        TrajectoryMatch(
            name="tool-check",
            mode="subset",
            expected_tools=["search", "summarize"]
        ),
        LLMJudge(
            name="quality",
            prompt="Was the agent's response helpful and accurate?",
            scoring="continuous"
        )
    ]
)

for r in results:
    print(f"{r.name}: {'PASS' if r.passed else 'FAIL'} ({r.score:.2f})")
```

## Example: Using the TypeScript SDK

```typescript
import { evaluate, TrajectoryMatch, LLMJudge } from "agentevals";

const trace = await loadTrace("./traces/my-agent.json");

const results = await evaluate({
  trace,
  evaluators: [
    new TrajectoryMatch({
      name: "tool-check",
      mode: "subset",
      expectedTools: ["search", "summarize"],
    }),
    new LLMJudge({
      name: "quality",
      prompt: "Was the agent's response helpful and accurate?",
      scoring: "continuous",
    }),
  ],
});

results.forEach((r) => {
  console.log(`${r.name}: ${r.passed ? "PASS" : "FAIL"} (${r.score.toFixed(2)})`);
});
```
