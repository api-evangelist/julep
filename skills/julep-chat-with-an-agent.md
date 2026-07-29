---
name: Chat with a Julep agent
description: Create an agent, open a stateful session, and exchange chat messages that persist across turns.
api: openapi/julep-openapi-original.yml
operations: [AgentsRoute_create, SessionsRoute_create, ChatRoute_generate]
---

# Chat with a Julep agent

Use this skill to stand up a conversational Julep agent and chat with it in a session that remembers history.

## Auth
Send your API key on every request as `Authorization: <api_key>` (or `X-Auth-Key: <api_key>`). Base URL: `https://api.julep.ai/api`.

## Steps
1. **Create an agent** — `POST /agents` (`AgentsRoute_create`). Provide `name`, `model`, and optional `instructions`. Capture the returned agent `id`.
2. **Create a session** — `POST /sessions` (`SessionsRoute_create`). Reference the agent (`agent`) and optionally a `user`. Capture the session `id`. Sessions hold conversation state, so reuse the same session id across turns.
3. **Send a chat message** — `POST /sessions/{id}/chat` (`ChatRoute_generate`) with the session id and a `messages` array. The response contains the agent reply; history is persisted automatically.

## Conventions
- Pagination is cursor-based via `next_token` (see `conventions/julep-conventions.yml`).
- The spec declares no problem+json error envelope; handle standard HTTP status codes (401 invalid key, 404 unknown id, 422 validation).
- No idempotency-key contract — do not assume safe retries on POST.
