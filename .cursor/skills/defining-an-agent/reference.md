# Agent definition reference

Apply every section this system touches.

## From prediction to an agent

A model that answers one prompt is predictive. An agent keeps going until the goal is met or a stop condition fires. Say which of the two you are building.

## Problem-solving process

The loop is thought, action, observation, then either another thought or a final answer. The orchestration layer notices when a stage of the plan is finished. A hidden chain of guesses with no observation is not the loop.

## Capability levels

| Level | What it can do | Build this when |
| --- | --- | --- |
| 0 Core reasoning | The model only | The task is one-shot generation |
| 1 Connected problem-solver | Tools, one action at a time | The answer depends on live data or a single action |
| 2 Strategic problem-solver | A multi-step plan and task context | The goal needs a sequence the model chooses |
| 3 Collaborative multi-agent | Specialists and a coordinator | One instruction set cannot hold the job |
| 4 Self-evolving | The system creates or changes its own agents | The user asked for that research target |

Production work stops at level 3 unless the user asks for level 4.

## Model, the brain

Select the model as an architecture choice. Keep a path to swap it when a newer model wins your metrics. Do not freeze the model into the design.

## Tools, the hands

- Retrieve: search, retrieval over documents, natural language to SQL.
- Act: wrap an existing API, or generate code and run it in a sandbox.
- Function calling is how the model binds a decision to a tool schema. The schema is the contract.

## Orchestration

- Autonomy: how much the model may choose versus a fixed workflow.
- Instruction: domain knowledge and persona live here, once.
- Context: assembled at runtime. See `engineering-agent-context`.
- Multi-agent: a team of specialists beats one agent that does every job. See `choosing-agent-architecture`.

## Deployment and services

A local runner is a prototype. A service is a deployed agent with an identity, a session store, and a way to turn it off. See `deploying-agents`.

## Agent Ops

| Practice | Rule |
| --- | --- |
| Measure what matters | Score the business outcome, as in an experiment, not "it replied" |
| Quality, not pass/fail | An LM judge scores the trajectory and the answer |
| Metrics-driven release | A threshold is the go or no-go |
| Traces | OpenTelemetry-style traces answer why a run failed |
| Human feedback | Store it and use it. A comment that never re-enters the eval set is unused |

## Interoperability

| With | Rule |
| --- | --- |
| Humans | A UI, plus confirmation when the action is hard to undo |
| Agents | A2A. See `connecting-agents` |
| Money | A payment is a separate policy. A tool does not spend money because the model asked |

## Security of one agent

- Trust tradeoff: each new tool widens both capability and blast radius.
- Identity: the agent is its own principal. Do not silently reuse the user's credentials as if they were the agent's.
- Policies: constrain which tools and which data that principal may touch.
- An ADK agent is hardened with a tool allowlist, confirmation on irreversible acts, and secrets kept out of the prompt.

## A fleet

- Harden privacy and security at fleet scale, not only inside one prompt.
- Governance is a control plane: ownership, a registry, and policy. A folder of unowned agents is sprawl.

## How agents change

- They improve through evaluation and memory, not through a longer prompt alone.
- A simulation or agent gym runs the task suite before users do.
- Co-Scientist and AlphaEvolve are level-4 research examples. They are not a template for a product agent.
