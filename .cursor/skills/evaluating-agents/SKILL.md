---
name: evaluating-agents
description: Use when scoring agent quality, building an eval set, or judging tool trajectory, response match, an LLM judge, or agent failure modes.
---

# Evaluating agents

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. Grade this agent's trajectory, not only the last sentence. A tool that passes its unit tests can still be the wrong tool for the turn.

## Failure modes to name

| Mode | What it looks like |
| --- | --- |
| Algorithmic bias | The agent treats people unequally because the data or the rubric did |
| Factual hallucination | A fluent answer that is not in the tools or the source |
| Drift | Quality moves because the model, the tools, or the world moved |
| Unintended behavior | A novel path that was never in the spec, including unsafe tool use |

Say which of these this agent can hit.

## Build the set

Capture a session from the ADK web UI, or write the case. Each case has the user text, the expected final response, and the expected tool calls.

```python
eval_config = {
    "criteria": {
        "tool_trajectory_avg_score": 1.0,
        "response_match_score": 0.8,
    }
}
```

`tool_trajectory_avg_score` of 1.0 means the tool name and arguments must match. `response_match_score` is text similarity, not proof of truth. Add an LM judge when similarity cannot see a correct paraphrase.

```bash
adk eval home_automation_agent home_automation_agent/integration.evalset.json \
  --config_file_path=home_automation_agent/test_config.json \
  --print_detailed_results
```

Read the per-turn diff. A failed trajectory is a wrong tool or a wrong argument. A failed response match is wording or a missing fact.

## Who judges

| Judge | Role |
| --- | --- |
| Automated metric | Trajectory match, similarity, latency, schema validity |
| LLM-as-a-judge | A rubric the model applies to the trajectory |
| Agent-as-a-judge | A second agent that checks a specialist's work |
| User feedback | The person, through a review UI, who owns the definition of good |

The human writes the rubric. The automated judge applies it at scale. A judge without a human rubric is unanchored.

User simulation is optional: another model plays the user across turns. Use it to find paths a fixed script never asks.

## Flywheel

1. Instrument so the run is evaluable. See `observing-agents`.
2. Score the trajectory against the golden set.
3. Block release when the score drops. See `deploying-agents`.
4. Turn a production failure into a new case in the golden set.

## Principles

- Evaluation is part of the design. Add the trace points while you write the agent.
- The trajectory is the truth. The final sentence is the last event.
- The human is the arbiter of what a good score means.

## Done when

This agent's eval set has at least one case with expected tools and an expected answer, the thresholds are numbers, and the command has been run or handed to the person with the path filled in.
