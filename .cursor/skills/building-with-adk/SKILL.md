---
name: building-with-adk
description: Use when creating or running a Google ADK agent, including Gemini, a Runner, retry on 429, google_search, or the ADK web UI.
---

# Building with ADK

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation.

Install with `pip install google-adk`. Put the API key in `GOOGLE_API_KEY`. If the environment is Kaggle, that key is a secret named `GOOGLE_API_KEY`.

Space the calls. A burst of requests returns 429.

## Retry

```python
from google.genai import types

retry_config = types.HttpRetryOptions(
    attempts=5,
    exp_base=7,
    initial_delay=1,
    http_status_codes=[429, 500, 503, 504],
)
```

Pass `retry_options=retry_config` on every `Gemini(...)`.

## One agent

```python
from google.adk.agents import Agent
from google.adk.models.google_llm import Gemini
from google.adk.runners import InMemoryRunner
from google.adk.tools import google_search

root_agent = Agent(
    name="helpful_assistant",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    description="Answers general questions.",
    instruction="You are a helpful assistant. Use Google Search for current info or if unsure.",
    tools=[google_search],
)
runner = InMemoryRunner(agent=root_agent)
response = await runner.run_debug("What is Agent Development Kit from Google?")
```

`run_debug` creates a temporary session. It is for a prototype. Real sessions are `managing-agent-sessions`.

The agent uses a tool because the tool is in `tools` and the instruction says when to use it. Confirm both.

`adk run`, `adk web`, and `adk api_server` are the other ways to run the same agent.

## ADK web UI

`adk create sample-agent --model gemini-2.5-flash-lite --api_key $GOOGLE_API_KEY` writes `agent.py`, `__init__.py`, and `.env`.

Then `adk web`. If the environment is Kaggle, open the proxy URL only after that process is running. That URL contains an auth token. Do not share it.

The sample agent from `adk create` has no tools until you add them.

## Done when

This agent has a name, a model, an instruction, a tool list, a retry config, and one successful `run_debug` or `adk web` turn.
