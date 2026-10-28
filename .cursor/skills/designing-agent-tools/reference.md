# Tool catalog and design rules

## Custom tools

| Kind | Use |
| --- | --- |
| Function tool | A Python function on `tools`. ADK builds the schema. |
| Long-running function | The work pauses for a person or a slow job. Needs a resumable App. |
| Agent tool | A specialist the caller invokes and then continues from. |
| MCP tool | A server's tools, via `using-mcp`. |
| OpenAPI tool | Generate tools from an API spec instead of hand-writing one client per route. |

## Built-in tools

| Kind | Examples |
| --- | --- |
| Gemini | `google_search`, `BuiltInCodeExecutor` |
| Google Cloud | `BigQueryToolset`, `SpannerToolset`, `APIHubToolset`. These need a cloud project. |
| Third party | Existing ecosystems such as GitHub, Firecrawl, or Hugging Face wrappers |

## Design rules

- Document the action, the arguments, the side effects, and the return keys. The docstring is what the model reads.
- Describe the user's task in the instruction ("file a bug for this issue"). Do not name the function as the procedure, and do not copy the docstring into the instruction.
- State a side effect that another tool depends on, such as a fetch that stores a page for a later tool.
- One tool, one responsibility. A multi-step tool is allowed only when that sequence is the common path and the docstring says every step.
- Return a small payload. Put large tables, files, and images in an external store or ADK's artifact service, and return the handle.
- Validate inputs and outputs against the schema at runtime. The schema is also documentation.
- On failure, return an `error_message` that tells the model the next step, not only an error code.

## Built-in versus agent-written tools

A built-in tool is ready and shared. An agent tool is your business rule. Prefer a built-in tool when it already does the task. Write a function when the rule is yours.
