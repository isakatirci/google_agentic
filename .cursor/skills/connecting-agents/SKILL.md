---
name: connecting-agents
description: Use when agents in separate services must collaborate, or when the task mentions A2A, an agent card, to_a2a, or RemoteA2aAgent.
---

# Connecting agents

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. A local sub-agent shares its process and its code. A remote agent belongs to someone else, or must deploy on its own. Talk to that agent with A2A. Call a function with MCP. Do not use one protocol for the other's job.

| You need | Use |
| --- | --- |
| A tool with a schema | MCP. See `using-mcp` |
| Another agent that plans and holds its own state | A2A |
| A specialist you ship in the same process | `AgentTool` or `sub_agents`. See `choosing-agent-architecture` |

## Expose

```python
from google.adk.a2a.utils.agent_to_a2a import to_a2a

product_catalog_a2a_app = to_a2a(product_catalog_agent, port=8001)
```

`to_a2a` serves the agent and writes an agent card at `/.well-known/agent-card.json`. The card lists name, description, skills, and endpoints. Run the server as its own process.

## Consume

```python
from google.adk.agents.remote_a2a_agent import RemoteA2aAgent, AGENT_CARD_WELL_KNOWN_PATH

remote_product_catalog_agent = RemoteA2aAgent(
    name="product_catalog_agent",
    description="Remote catalog agent from the vendor.",
    agent_card=f"http://localhost:8001{AGENT_CARD_WELL_KNOWN_PATH}",
)
customer_support_agent = LlmAgent(
    name="customer_support_agent",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Look up products only through product_catalog_agent, then answer.",
    sub_agents=[remote_product_catalog_agent],
)
```

`RemoteA2aAgent` reads the card and sends the task over HTTP. The caller writes it like a sub-agent. The protocol stays inside ADK.

The path of one question: the support agent decides it needs a fact, calls the remote agent, the catalog agent uses its own tools, the result returns, the support agent answers.

## When a registry exists

Build a tool registry when people can no longer find tools, or security must audit them in one place. Build an agent registry when teams must discover each other's agents. Under that scale, a list in a config file is enough.

Generalist agents see a wide catalog and lose accuracy. Specialists see a curated subset. Dynamic agents query the registry at runtime. Prefer specialists until discovery is the bottleneck.

## Done when

This agent has a card URL for the remote agent, uses `RemoteA2aAgent` or an equivalent client, and does not share a private event log in place of a task.
