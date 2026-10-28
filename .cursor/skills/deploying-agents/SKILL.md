---
name: deploying-agents
description: Use when moving this agent from a local run to production, including Agent Engine, Cloud Run, GKE, CI/CD, rollout, AgentOps, or Memory Bank on a deployment.
---

# Deploying agents

Use terms from `GLOSSARY.md`.

The subject is the agent in this conversation. No version of this agent reaches users without an evaluation that beats or holds the current baseline. That is evaluation-gated deployment.

## Before a cloud deploy

Use a Google Cloud project with billing. Enable Vertex AI, Cloud Storage, Cloud Logging, Cloud Monitoring, Cloud Trace, and Telemetry. Set `GOOGLE_CLOUD_PROJECT` to that project's id. Authenticate with application default credentials. If the environment is Kaggle, link the Cloud SDK account and read the credential from user secrets.

## Package

A deployable agent is a directory.

- `agent.py` exports `root_agent`.
- Dependencies are pinned in requirements.
- The environment chooses the backend on purpose.

```python
# Vertex AI, not AI Studio
# GOOGLE_GENAI_USE_VERTEXAI=1
# GOOGLE_CLOUD_PROJECT=...
# GOOGLE_CLOUD_LOCATION=global
vertexai.init(
    project=os.environ["GOOGLE_CLOUD_PROJECT"],
    location=os.environ["GOOGLE_CLOUD_LOCATION"],
)
```

`GOOGLE_GENAI_USE_VERTEXAI=1` selects Vertex. `0` selects AI Studio. Do not leave this implicit.

Tools in the deployed file follow `designing-agent-tools`: a docstring, a status dictionary, no secrets in source.

## Pick a runtime

| Runtime | Choose it when |
| --- | --- |
| Vertex AI Agent Engine | You want a managed agent runtime, sessions, and scale |
| Cloud Run | You want a small service and the least infrastructure |
| GKE | You need control of a multi-agent cluster |

Agent Engine deploy:

```bash
adk deploy agent_engine --project=$PROJECT_ID --region=$REGION sample_agent \
  --agent_engine_config_file=sample_agent/.agent_engine_config.json
```

```json
{
  "min_instances": 0,
  "max_instances": 1,
  "resource_limits": {"cpu": "1", "memory": "1Gi"}
}
```

`min_instances` of 0 scales to zero. Pick a region near the users that also meets data-residency rules.

Call the deployed agent, then delete a test deploy so it does not keep billing:

```python
from vertexai import agent_engines

vertexai.init(project=PROJECT_ID, location=REGION)
remote_agent = list(agent_engines.list())[0]
async for item in remote_agent.async_stream_query(
    message="What is the weather in Tokyo?",
    user_id="user_42",
):
    print(item)
agent_engines.delete(resource_name=remote_agent.resource_name, force=True)
```

`list()` order is not a contract. Delete by the resource name you just deployed, not by "whatever is first" once more than one agent exists.

Enable Memory Bank on that deployment only if the agent must remember across sessions: add the memory tool and the save callback, then redeploy. See `persisting-agent-memory`.

## Gate, pipeline, rollout

The full loop is in [reference.md](reference.md). The minimum:

1. Run the golden eval set before merge, and put the result where a reviewer can see it.
2. Treat prompts, tool definitions, and config as release artifacts, not only Python.
3. Roll out to a fraction of traffic before everyone.
4. Watch traces in production and add each new failure to the golden set.

## Done when

This agent's directory is complete, its runtime is named, the eval result exists for this version, and a test deploy can scale to zero or be deleted.
