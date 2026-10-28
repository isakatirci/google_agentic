---
name: observing-agents
description: Use when an agent fails and the cause is unclear, or when adding logs, traces, metrics, callbacks, or LoggingPlugin.
---

# Observing agents

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. A wrong answer without a trajectory is a guess. Read this agent's path before changing the prompt.

## Three records

| Record | Question it answers |
| --- | --- |
| Log | What happened at one moment |
| Trace | Why the final result happened, as the ordered steps |
| Metric | How the system is doing, as a rate or a latency |

Monitoring tells you that something is wrong. Observability lets you reconstruct the turn.

## See one failure

1. Turn logging to DEBUG for the run you are debugging.
2. Open the ADK web UI Events tab, or read the local log.
3. Find the model request, the tool list, the tool call, and the tool result.
4. Name the broken step. A common one is an empty tool list, or a tool argument of the wrong type.

A wrong type is a common break: a tool declared `papers: str` while the instruction passed a list. The trace shows the bad call. Fix the type to `List[str]`.

Stop `adk web` before you start another server on the same port.

## Production

A callback on one agent sees that agent. A plugin registered on the Runner sees every agent, model call, and tool call.

```python
from google.adk.plugins.logging_plugin import LoggingPlugin

runner = InMemoryRunner(
    agent=research_agent,
    plugins=[LoggingPlugin()],
)
```

`LoggingPlugin` records user messages, responses, timing, model requests, and tool calls. Write a `BasePlugin` only when you need a count or a policy the built-in plugin does not record. Hooks include `before_agent_callback` and `before_model_callback`.

What a trace should contain: the input, the instruction identity, each tool name and arguments, each tool result, the model output, and the latency of those steps.

## Metrics

| Kind | Examples |
| --- | --- |
| System | Latency, error rate, token count, tool-call count |
| Quality | Task success, trajectory match, refusal rate |

A dashboard without a trace still cannot answer why one user failed. Keep both.

## Done when

This agent's failing turn has a named step, the evidence is a log line or a trace event, and the Runner that will keep running has `LoggingPlugin` or an equivalent plugin.
