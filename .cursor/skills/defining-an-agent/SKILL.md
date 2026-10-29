---
name: defining-an-agent
description: Use when deciding whether a system is an agent, placing it on the capability levels, or designing its model, tools, orchestration, Agent Ops, identity, or security.
---

# Defining an agent

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. An agent plans, acts, and observes. A prompt that only returns text is level 0.

## Steps

1. Name this agent's level from [reference.md](reference.md). Done when the level and the missing capability are both stated.
2. Name this agent's model, tools, and orchestration. Done when each is specified or explicitly absent.
3. Route work across models on purpose: a stronger model plans, a faster model classifies high-volume turns, a multimodal model or specialist API covers images and audio. Done when one path is picked and named.
4. Put retrieval and actions in tools. Ground answers in search, documents, or a query; change the world through an API or sandboxed code. Done when each capability is named as a tool or marked absent.
5. Apply the Agent Ops and security rows in [reference.md](reference.md) that this agent touches. Done when each touched row is decided, not deferred.

Workflow shape belongs to `choosing-agent-architecture`. ADK wiring belongs to `building-with-adk`.

## Done when

This agent has a level, a model, a tool list, and an orchestration choice. Each Agent Ops or security row it touches is decided.
