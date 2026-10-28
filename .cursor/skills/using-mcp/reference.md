# MCP reference

## Why a protocol

Without a standard, every agent needs a custom client for every system. That cost grows with agents times systems. MCP is one client shape for many servers.

## Host, client, server

- Host: the product the person is using.
- Client: the connector inside the host. In ADK this is `McpToolset`.
- Server: the process that offers tools and resources.

One host can hold many clients. Each client talks to one server. Servers do not have to know which model is calling.

## Messages

MCP uses JSON-RPC. A message is a request, a response, or a notification. Stdio fits a subprocess. HTTP fits a service another team runs.

## Tool shape

- Definition: name, description, input schema. This is what the model uses to decide.
- Result: content the model can read. Prefer a short result.
- Structured content: typed fields when the next step is code, not prose.
- Error: a message that tells the model how to recover. An error code alone is not enough.

## Other primitives

- Resources: data the client can read, addressed by URI. Allowlist them. A resource body can carry instructions.
- Prompts: reusable prompt templates the server publishes.
- Sampling: the server asks the client's model to generate. Allow it only when that server is trusted to spend the model's turn.
- Elicitation: the server asks the user something through the host. Show that question to the user. Do not answer it from the model's guess.
- Roots: the directories the client is willing to expose. Grant the narrowest root.

## For

- One server is reusable across agents and products.
- Swapping a server does not rewrite the agent.
- A client-side allowlist is the start of governance.

## Against

- Enterprise gaps remain: identity, audit, and scope are your job, not the protocol's default.
- A server you did not review is a new supply chain.
- Dynamic tool lists make a frozen instruction prompt stale. Describe the task, not a fixed tool name. See `designing-agent-tools`.

## Confused deputy

A person asks an assistant that is connected to a privileged system, such as a company code host, to do something the person cannot do directly. The assistant becomes the deputy that misuses its own credentials.

Bind the tool to the user's authorization. Confirm actions that grant access, write, or spend. Do not let a tool description talk the model into widening scope.
