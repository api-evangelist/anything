---
name: Provision and query a database
description: Create a managed PostgreSQL database for a project and run read-only queries against it.
api: openapi/anything-openapi-original.json
generated: '2026-07-17'
method: generated
source: openapi/anything-openapi-original.json + conventions/anything-conventions.yml
operations:
  - "POST /v0/api/databases"
  - "GET /v0/api/databases/{databaseId}"
  - "POST /v0/api/databases/{databaseId}/query"
  - "GET /v0/api/databases/{databaseId}/connection"
---

# Provision and query a database

Authenticate with HTTP Basic auth (API key as username, empty password).

## Steps

1. **Create the database** — `POST /v0/api/databases`. Provisioning is async.
2. **Poll until ready** — `GET /v0/api/databases/{databaseId}` until `status`
   leaves `CREATING`.
3. **Query** — `POST /v0/api/databases/{databaseId}/query`. Only read-only
   statements are allowed (`SELECT`, `WITH`, `EXPLAIN`, `SHOW`).
4. **Connection string** — `GET /v0/api/databases/{databaseId}/connection` when you
   need to connect a client directly.

## Conventions

- List endpoints paginate with `limit` (max 100) and `offset`.
- Deleting a database (`DELETE /v0/api/databases/{databaseId}`) soft-deletes with a
  30-day grace period before permanent removal.
- Errors: JSON `{error, details, message}` envelope — see `errors/anything-problem-types.yml`.
