# Production reference

## People

Shipping an agent takes more than the person who wrote the prompt. Name who owns behavior, who owns the eval set, who owns the pipeline, and who can block a release. An unowned eval set will not stay a gate.

## Evaluation gate

Two ways to enforce it. The rule is the same: a failed or missing eval does not deploy.

| Gate | How |
| --- | --- |
| Pre-PR | The author runs the suite locally and links the report on the change. The reviewer checks behavior, not only the diff. |
| In the pipeline | The suite runs in CI. A score under the threshold blocks the deploy. |

Compare against the production baseline. Unit tests of tools do not count as this gate. See `evaluating-agents`.

## CI/CD

The pipeline is a funnel. Catch cheap failures first.

1. Pre-merge: install, static checks, and the fast eval slice.
2. Post-merge: the full golden set, including trajectory and safety cases.
3. Release: only a version that passed stage 2.

A prompt change is a release. So is a tool-schema change.

## Safe rollout

Ship to a small share of traffic first. Watch quality and system metrics. Widen the share only if those hold. Keep the previous version runnable so you can return to it.

## Security from the start

- Secrets come from the environment or a secret manager.
- Tool allowlists are part of the release, not a later review.
- Confirm actions that write, pay, or grant access. See `designing-agent-tools`.
- Assume tool text and retrieved documents can contain instructions. See `using-mcp`.

## Operate

| Lever | Watch |
| --- | --- |
| Performance | Latency of the model and of each tool |
| Cost | Tokens, tool calls, instances that never scale down |
| Scale | Sessions and memory at more than one user |
| Security response | A playbook for a bad tool, a leaked secret, or a prompt injection: disable the tool or the version, then add a case to the golden set |

## Evolve

Production traces feed the next eval case. The next change goes through the same gate and the same rollout. That loop is AgentOps: local prototype, pre-production gate, safe release, observe, then the next change.

## A2A and MCP in production

MCP reaches tools. A2A reaches other agents. A system can use both: agents delegate to agents over A2A, and each agent calls its own tools over MCP. See `connecting-agents`.

Build a tool or agent registry when discovery or audit exceeds what a config file can hold. Until then, a short allowlist is the registry.
