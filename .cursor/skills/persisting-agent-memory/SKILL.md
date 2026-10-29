---
name: persisting-agent-memory
description: Use when an agent must remember facts across sessions, including load_memory, preload_memory, MemoryService, consolidation, or Memory Bank.
---

# Persisting agent memory

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. A session ends. Memory is what a later session of this agent can still retrieve. Putting `memory_service` on the Runner only makes the store available. You still ingest and you still give the agent a retrieval tool.

## Choose a service

| Service | Search | Leaves the process |
| --- | --- | --- |
| `InMemoryMemoryService` | Keywords over raw events | No |
| `VertexAiMemoryBankService` | Semantic, after consolidation | Yes |
| Your own `BaseMemoryService` | Whatever you implement | If you built it that way |

The calls are the same: `add_session_to_memory(session)` and `search_memory`.

## Ingest, then retrieve

```python
session = await session_service.get_session(
    app_name=APP_NAME, user_id=USER_ID, session_id="conversation-01"
)
await memory_service.add_session_to_memory(session)
```

```python
user_agent = LlmAgent(
    name="MemoryDemoAgent",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Answer simply. Use load_memory when the answer may be in a past conversation.",
    tools=[load_memory],
)
runner = Runner(
    agent=user_agent,
    app_name=APP_NAME,
    session_service=session_service,
    memory_service=memory_service,
)
```

| Tool | Behavior |
| --- | --- |
| `load_memory` | The model searches when it decides to. Cheaper. It can forget to search. |
| `preload_memory` | A search runs before every turn. The fact is present. Tokens are spent even when the fact is irrelevant. |

Use `load_memory` unless a missed memory is worse than the extra tokens.

## Save without a manual call

```python
async def auto_save_to_memory(callback_context):
    await callback_context._invocation_context.memory_service.add_session_to_memory(
        callback_context._invocation_context.session
    )

auto_memory_agent = LlmAgent(
    name="AutoMemoryAgent",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Answer user questions.",
    tools=[preload_memory],
    after_agent_callback=auto_save_to_memory,
)
```

| When to ingest | Use |
| --- | --- |
| After every turn | `after_agent_callback` |
| When the conversation ends | One call at the end |
| On a timer | A background job for a long session |

Other callback points: `before_agent_callback`, `before_tool_callback`, `after_tool_callback`, `before_model_callback`, `after_model_callback`, `on_model_error_callback`.

## Consolidation

Raw storage keeps every message and every tool call. Search then returns that bulk. Consolidation extracts facts, merges duplicates, and drops chatter. `InMemoryMemoryService` does not consolidate. Memory Bank does. Use the bank when the store must stay small and meaningful.

Design of kinds, provenance, and privacy is in `engineering-agent-context`.

## Done when

A fact said to this agent in one session is answered in a new session only because memory was ingested and a retrieval tool ran, and a different user cannot retrieve it.
