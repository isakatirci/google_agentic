# Context reference

The ADK steps for sessions and memory are `managing-agent-sessions` and `persisting-agent-memory`. This file is the design those steps implement.

## Sessions across systems

- One session is one conversation, one user, one agent.
- A multi-agent workflow may use one shared state for keys, and separate instructions per agent.
- Another vendor's agent does not read your event table. Give it a task. Take back a result.

## Production sessions

- In-memory dies on restart. A database or Agent Engine session survives.
- Isolate users. A session id collision is a data leak.
- Cap growth with compaction. Unbounded events become the next turn's bill.

## Memory design

| Choice | Options |
| --- | --- |
| Kind | Episodic events, semantic facts, procedural how-to |
| Information | Preferences, entities, commitments, raw dialogue |
| Organization | By user, by app, by topic |
| Storage | Process memory, a database, a managed bank |
| Creation | Extract on a trigger, then consolidate |
| Scope | This user, this app, this agent |
| Modality | Text is the default. Store a pointer for audio or images, not the blob in the prompt |

## Extraction and consolidation

Extraction pulls candidate facts from a session. Consolidation merges duplicates, drops chatter, and resolves contradictions. A store that keeps every raw turn will return the whole turn on search. Prefer a service that consolidates, such as Vertex AI Memory Bank, once you leave the prototype.

## Provenance

Keep where a fact came from and when. On write, do not let a new fact silently overwrite a conflicting one without a rule. On read, prefer the fact whose source and time still apply. A memory with no source is a rumor in the prompt.

## Triggers

Generate memory after a turn, at the end of a session, or on a timer. Match the trigger to how fresh the fact must be. See the timing table in `persisting-agent-memory`.

## Memory as a tool

The agent searches memory by calling a tool. That keeps retrieval out of turns that do not need it. Preloading searches every turn and spends tokens to be sure.

## Retrieval timing

- Before the model call: guaranteed, paid on every turn.
- When the model asks: cheaper, and the model can forget to ask.
- After a tool, before the next thought: only when that tool's result says a lookup is required.

## Using a retrieved memory

- In the instruction: always-on preferences. Keep this short.
- In the history: evidence for this turn.
- As a procedure: a step the agent should follow, stored as a skill or a tool, not as a paragraph of chat.

## Test and protect

- Test that a fact saved in session A is retrievable in session B, and that an unrelated user cannot retrieve it.
- Memories are personal data. Scope them to the user, delete them on request, and do not write secrets into the store.
