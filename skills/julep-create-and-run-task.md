---
name: Create and run a Julep task
description: Define a multi-step task on an agent, execute it, and stream execution status to completion.
api: openapi/julep-openapi-original.yml
operations: [AgentsRoute_create, TasksRoute_create, TaskExecutionsRoute_create, ExecutionStatusStreamRoute_stream, ExecutionsRoute_get]
---

# Create and run a Julep task

Use this skill to define a declarative multi-step workflow (decisions, loops, parallel branches, tool calls) and run it.

## Auth
Send `Authorization: <api_key>` on every request. Base URL: `https://api.julep.ai/api`.

## Steps
1. **Create an agent** — `POST /agents` (`AgentsRoute_create`). Capture the agent `id`.
2. **Create a task on the agent** — `POST /agents/{id}/tasks` (`TasksRoute_create`) with the task definition (`name`, `main` steps). Capture the task `id`.
3. **Start an execution** — `POST /tasks/{id}/executions` (`TaskExecutionsRoute_create`) with the task id and an `input` object. Capture the execution `id`.
4. **Stream status** — `GET /executions/{id}/status.stream` (`ExecutionStatusStreamRoute_stream`) as an SSE (`text/event-stream`) connection to watch state transitions in real time.
5. **Fetch the result** — `GET /executions/{id}` (`ExecutionsRoute_get`) once the stream reports a terminal state (`succeeded` / `failed` / `cancelled`) to read the final output.

## Conventions
- Executions progress through transitions; list them with `GET /executions/{id}/transitions`.
- SSE endpoints emit `text/event-stream`; keep the connection open until a terminal transition.
- See `data-model/julep-data-model.yml` for the Agent → Task → Execution → Transition graph.
