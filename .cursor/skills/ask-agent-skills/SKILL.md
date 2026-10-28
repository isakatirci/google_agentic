---
name: ask-agent-skills
description: Router for the agent craft commands. Names which slash command to type.
disable-model-invocation: true
---

# Ask the agent skills

Use terms from `GLOSSARY.md`.

This skill only names the commands. It does not run them. Type the one that matches the work in front of you.

Rules below are adapted from the Apache 2.0 Kaggle 5-day Agents notebooks, Copyright 2025 Google LLC. They are operating rules for an agent you are building, not a course to finish.

## Create

| You are about to | Type |
| --- | --- |
| Decide what the agent is | `/design-agent` |
| Pick a workflow shape | `/choose-agent-workflow` |
| Write and run the ADK agent | `/scaffold-adk-agent` |
| Add or review tools | `/add-agent-tools` |
| Call tools on an MCP server | `/connect-mcp` |

## Develop

| You are about to | Type |
| --- | --- |
| Decide the context payload | `/engineer-agent-context` |
| Keep one conversation going | `/add-agent-session` |
| Remember facts after the session | `/add-agent-memory` |
| Explain a failure | `/trace-agent` |
| Score the trajectory | `/eval-agent` |

## Ship

| You are about to | Type |
| --- | --- |
| Call an agent in another service | `/connect-a2a` |
| Gate and deploy | `/ship-agent` |
