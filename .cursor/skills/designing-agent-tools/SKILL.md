---
name: designing-agent-tools
description: Use when adding or reviewing agent tools, including function tools, AgentTool, code execution, OpenAPI tools, long-running tools, or human approval.
---

# Designing agent tools

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. A tool is a task this agent performs for the user. It is not a thin wrapper over an API method.

## Steps

1. Pick the tool kind from [reference.md](reference.md). Done when the kind is named.
2. Write the function so the model can call it: type hints, a docstring that says when to call it, and a dictionary return of `{"status": "success", ...}` or `{"status": "error", "error_message": ...}`.
3. Put the function on `tools`. Mention the task in the instruction. Do not restate the docstring, and do not hard-code a tool sequence when the model should choose.
4. Move exact work out of the model. Math and parsing go to code: `BuiltInCodeExecutor` on a specialist, or ordinary Python inside the tool.
5. If the action can wait on a person, follow the long-running section. Done when a small case returns immediately and a large case pauses and resumes.

## Function tool

```python
def get_fee_for_payment_method(method: str) -> dict:
    """Look up the transaction fee percentage for a payment method.

    Args:
        method: Payment method name, for example "platinum credit card".
    Returns:
        {"status": "success", "fee_percentage": 0.02}
        or {"status": "error", "error_message": "..."}.
    """
    fee = {"platinum credit card": 0.02, "bank transfer": 0.01}.get(method.lower())
    if fee is None:
        return {"status": "error", "error_message": f"Payment method '{method}' not found"}
    return {"status": "success", "fee_percentage": fee}
```

The instruction tells the agent to read `status` and stop on `error`.

## Agent as a tool

```python
calculation_agent = LlmAgent(
    name="CalculationAgent",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Reply with only a Python block that prints the result. Do not calculate it yourself.",
    code_executor=BuiltInCodeExecutor(),
)
# tools=[..., AgentTool(agent=calculation_agent)]
```

`AgentTool`: the caller receives the result and continues. `sub_agents`: the specialist takes the conversation. Use `AgentTool` for a subtask.

## Long-running approval

`tool_context: ToolContext` is injected by ADK. Do not ask the model to pass it.

```python
def place_shipping_order(num_containers: int, destination: str, tool_context: ToolContext) -> dict:
    """Place a shipping order. Orders over 5 containers need approval."""
    if num_containers <= 5:
        return {"status": "approved", "order_id": f"ORD-{num_containers}-AUTO",
                "num_containers": num_containers, "destination": destination}
    if not tool_context.tool_confirmation:
        tool_context.request_confirmation(
            hint=f"Approve {num_containers} containers to {destination}?",
            payload={"num_containers": num_containers, "destination": destination},
        )
        return {"status": "pending", "message": "Approval required"}
    if tool_context.tool_confirmation.confirmed:
        return {"status": "approved", "order_id": f"ORD-{num_containers}-HUMAN",
                "num_containers": num_containers, "destination": destination}
    return {"status": "rejected", "message": "Order rejected"}
```

Wrap the agent so the pause can resume:

```python
shipping_app = App(
    name="shipping_coordinator",
    root_agent=shipping_agent,
    resumability_config=ResumabilityConfig(is_resumable=True),
)
# Runner(app=shipping_app, ...)  — pass the app, not only the agent
```

The first large-order call returns `pending` and emits a function call named `adk_request_confirmation`. The workflow must resume that same invocation. A new run starts a new invocation and the pause is lost.

```python
def check_for_approval(events):
    for event in events:
        if not (event.content and event.content.parts):
            continue
        for part in event.content.parts:
            if part.function_call and part.function_call.name == "adk_request_confirmation":
                return {"approval_id": part.function_call.id, "invocation_id": event.invocation_id}
    return None

def create_approval_response(approval_info, approved: bool):
    confirmation = types.FunctionResponse(
        id=approval_info["approval_id"],
        name="adk_request_confirmation",
        response={"confirmed": approved},
    )
    return types.Content(role="user", parts=[types.Part(function_response=confirmation)])
```

Send that content back on the saved `invocation_id`. The tool then reads `tool_confirmation.confirmed`.

## Done when

Each tool on this agent has one task, a schema, a short result, and an error string that tells the model what to do next.
