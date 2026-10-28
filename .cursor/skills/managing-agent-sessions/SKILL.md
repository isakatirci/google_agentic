---
name: managing-agent-sessions
description: Use when a conversation must continue across turns, or when choosing a SessionService, session state, or events.
---

# Managing agent sessions

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. A model call is stateless. A session is the record of one conversation. Events are the lines. State is the scratchpad. The SessionService is the cabinet. The Runner is what writes the next line.

## Choose a service

| Service | Survives restart | Use |
| --- | --- | --- |
| `InMemorySessionService` | No | A prototype |
| `DatabaseSessionService` | Yes, in your database | An app you operate |
| Agent Engine sessions | Yes, managed | Production on Google Cloud |

```python
session_service = InMemorySessionService()
runner = Runner(agent=root_agent, app_name=APP_NAME, session_service=session_service)
```

```python
session_service = DatabaseSessionService(db_url="sqlite:///my_agent_data.db")
```

Same session id, same user, same app: the next turn can see the earlier events. A new session id starts blank. Restarting the process clears an in-memory service even if the id is reused.

## State

Tools receive `tool_context` and read or write `tool_context.state`.

| Prefix | Lifetime |
| --- | --- |
| none | This session |
| `temp:` | This invocation |
| `user:` | This user, across that user's sessions |
| `app:` | The whole app |

```python
def save_userinfo(tool_context: ToolContext, user_name: str, country: str) -> dict:
    """Record the user's name and country."""
    tool_context.state["user:name"] = user_name
    tool_context.state["user:country"] = country
    return {"status": "success"}
```

Use a prefix on purpose. An unprefixed key in a new session is empty. A `user:` key can appear in a later session for that same user. Do not store another user's data under `app:`.

Instructions may interpolate state with `{key}` when the workflow agent wrote an `output_key`. That is how sequential and parallel pipelines pass results.

## Events

Inspect `session.events` when you need to see what was stored. A compaction summary shows up as an event whose `actions.compaction` is set. Compaction itself is `engineering-agent-context`.

## Done when

This agent's SessionService is named, each session is keyed by app, user, and session id, and every state key has a prefix that matches how long the fact must live.
