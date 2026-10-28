# Glossary

Shared language for the 5-day agents course. Use these words in code, prompts, and reviews.

**Agent.** A system that plans, takes actions, and uses the results until a goal is met. A single prompt that returns text is not an agent.

**Model.** The reasoning core. The brain. It does not itself fetch live data or change the world.

**Tool.** A function the model can call. The hands. A tool either retrieves information or takes an action.

**Orchestration.** The loop that decides the next thought, call, or stop. The nervous system.

**Trajectory.** The full path of thoughts, tool calls, and observations that produced an answer. The answer is the last step of the trajectory, not the thing you grade alone.

**Session.** One continuous conversation for one user and one agent. It holds events and state. It is not shared with other users.

**Event.** One entry in a session: user input, model output, tool call, or tool result.

**State.** The session scratchpad of key-value facts the tools and sub-agents can read. Not the same as memory.

**Memory.** Facts kept after the session ends, retrieved on a later session.

**Context engineering.** Assembling the payload the model sees on this turn: instruction, history, state, memories, tool results, and retrieved documents.

**Compaction.** Replacing older events with a shorter summary so the session still fits the context window.

**AgentTool.** A specialist agent wrapped so the caller stays in control and receives the result back.

**Workflow agent.** A deterministic director: SequentialAgent, ParallelAgent, or LoopAgent. It does not decide the order with a prompt.

**MCP.** Model Context Protocol. The standard an agent uses to call tools and resources on an external server.

**A2A.** Agent2Agent. The standard one agent uses to delegate a task to another agent that runs as its own service.

**Agent card.** The JSON description an A2A agent publishes at `/.well-known/agent-card.json`.

**Agent Ops.** The discipline of measuring, tracing, judging, and gating agent changes the way production software is operated.

**Eval set.** A saved set of conversations with the expected final response and the expected tool calls.

**Golden set.** The curated eval set that a change must pass before users see it. Production failures get added to it.

**HITL.** Human in the loop. A person confirms an action, or a person writes the rubric an automated judge applies.
