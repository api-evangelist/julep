---
name: Ground a Julep agent with documents (RAG)
description: Attach documents to an agent and run hybrid/vector/text search over them for retrieval-augmented generation.
api: openapi/julep-openapi-original.yml
operations: [AgentsRoute_create, AgentDocsRoute_create, AgentsDocsSearchRoute_search]
---

# Ground a Julep agent with documents (RAG)

Use this skill to give an agent a private knowledge base and retrieve relevant passages.

## Auth
Send `Authorization: <api_key>` on every request. Base URL: `https://api.julep.ai/api`.

## Steps
1. **Create an agent** — `POST /agents` (`AgentsRoute_create`). Capture the agent `id`.
2. **Add a document** — `POST /agents/{id}/docs` (`AgentDocsRoute_create`) with `title` and `content`. Julep embeds and indexes the document. Add `metadata` to enable filtered bulk operations later.
3. **Search the documents** — `POST /agents/{id}/search` (`AgentsDocsSearchRoute_search`) with a search request. Julep supports text-only, vector, and hybrid search request bodies; choose the mode that fits the query and read `DocSearchResponse`.

## Conventions
- List existing docs with `GET /agents/{id}/docs` (cursor paginated via `next_token`).
- Pass `include_embeddings` when you need the raw vectors returned.
- Bulk-delete by metadata filter with `DELETE /agents/{id}/docs` (`AgentDocsRoute_deleteBulk`).
