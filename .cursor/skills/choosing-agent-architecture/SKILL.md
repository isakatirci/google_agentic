---
name: choosing-agent-architecture
description: Use when a task needs more than one agent, or when choosing a coordinator, SequentialAgent, ParallelAgent, or LoopAgent.
---

# Choosing an agent architecture

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. One specialist with a clear instruction is easier to debug than one agent that does every job. Add a second agent when the jobs have different tools or different instructions.

## Decide

| The work is | Use |
| --- | --- |
| One job, one tool set | A single agent |
| The model should pick which specialist runs | A coordinator whose tools are `AgentTool`s |
| A fixed order, each step needs the previous output | `SequentialAgent` |
| Independent jobs that can run together | `ParallelAgent`, then a sequential aggregator |
| Draft, critique, rewrite, until approved or a cap | `LoopAgent` inside a `SequentialAgent` |

A prompt that says "first do A, then B" is not a guarantee. When the order must hold, use a workflow agent.

## Coordinator

The caller stays in control. Each specialist's result comes back.

```python
research_agent = Agent(
    name="ResearchAgent",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Use google_search. Return 2-3 findings with citations.",
    tools=[google_search],
    output_key="research_findings",
)
summarizer_agent = Agent(
    name="SummarizerAgent",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Summarize {research_findings} as 3-5 bullets.",
    output_key="final_summary",
)
root_agent = Agent(
    name="ResearchCoordinator",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Call ResearchAgent, then SummarizerAgent, then answer the user.",
    tools=[AgentTool(research_agent), AgentTool(summarizer_agent)],
)
```

`AgentTool` returns control to the caller. `sub_agents` hands the conversation over. Use `AgentTool` when the caller must keep working with the result.

## Sequential

```python
root_agent = SequentialAgent(
    name="BlogPipeline",
    sub_agents=[outline_agent, writer_agent, editor_agent],
)
```

Pass data by `output_key` and read it as `{output_key}` in the next instruction. Order in `sub_agents` is the order of execution.

## Parallel, then aggregate

```python
parallel_research_team = ParallelAgent(
    name="ParallelResearchTeam",
    sub_agents=[tech_researcher, health_researcher, finance_researcher],
)
root_agent = SequentialAgent(
    name="ResearchSystem",
    sub_agents=[parallel_research_team, aggregator_agent],
)
```

Each parallel agent writes a distinct `output_key`. The aggregator reads those keys. Do not parallelize steps that depend on each other.

## Loop

```python
def exit_loop():
    """Call only when the critique is exactly APPROVED."""
    return {"status": "approved", "message": "Exiting refinement loop."}

story_refinement_loop = LoopAgent(
    name="StoryRefinementLoop",
    sub_agents=[critic_agent, refiner_agent],
    max_iterations=2,
)
root_agent = SequentialAgent(
    name="StoryPipeline",
    sub_agents=[initial_writer_agent, story_refinement_loop],
)
```

The critic writes `APPROVED` or concrete edits. The refiner calls `exit_loop` only on that exact signal. `max_iterations` is the cap. The loop does not stop because the prose looks finished.

## Done when

This agent's pattern is named, its agents and `output_key`s are listed, and a sentence says what would go wrong with the next-simpler pattern.
