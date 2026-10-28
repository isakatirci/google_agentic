---
name: engineering-agent-context
description: Use when the context window, compaction, caching, or the assembled prompt payload is the problem, or when the task says context engineering.
---

# Engineering agent context

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. That agent sees one payload per turn. Context engineering is deciding what is in it. A long transcript is not a memory design.

## Assemble the payload

List every block that will be sent:

1. System instruction and persona.
2. Session events, or their compaction summary.
3. Session state values the instruction interpolates, such as `{blog_outline}`.
4. Memories retrieved for this turn.
5. Tool schemas and the latest tool results.
6. Retrieved documents.

Done when each block is either included or named as excluded, with a reason.

## Rules

- Put stable policy in the instruction. Put turn-specific facts in state, tool results, or retrieved memory.
- A multi-agent system does not share one session history. Pass only the `output_key` the next agent needs.
- Sessions differ across frameworks. When two frameworks must interoperate, pass an explicit task and result, not a private event log. Agent-to-agent handoff is `connecting-agents`.
- Long context has a cost and a quality cost. Compact or cache before you raise the window.

## Compaction and cache

```python
research_app = App(
    name="research_app_compacting",
    root_agent=chatbot_agent,
    events_compaction_config=EventsCompactionConfig(
        compaction_interval=3,
        overlap_size=1,
    ),
)
```

`compaction_interval` is how many invocations between summaries. `overlap_size` is how many recent turns stay verbatim. Compaction adds a summary event. It does not erase the stored history. The next turn sees the summary.

A custom `SlidingWindowCompactor` is for a different summarizer or a different model. Context caching is for a static instruction you pay for once.

## Where a fact lives

| Fact | Put it in |
| --- | --- |
| How the agent must behave | Instruction |
| This conversation's scratch numbers | Session state |
| A fact needed next week | Memory. See `persisting-agent-memory` |
| How to do a recurring procedure | A procedural memory or a tool, not a longer chat |

## Done when

This agent's payload list exists, a compaction or caching choice is made for conversations that grow, and no block is present only because it was easy to append.
