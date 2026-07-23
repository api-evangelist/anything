---
name: Create and launch an app
description: Use the Anything API to create a new project from a prompt, iterate on it, and publish it live.
api: openapi/anything-openapi-original.json
generated: '2026-07-17'
method: generated
source: openapi/anything-openapi-original.json + conventions/anything-conventions.yml
operations:
  - "POST /v0/api/projects"
  - "GET /v0/api/projects/{projectGroupId}/status"
  - "POST /v0/api/projects/{projectGroupId}/generate"
  - "POST /v0/api/projects/{projectGroupId}/publish"
---

# Create and launch an app

Authenticate every request with HTTP Basic auth: send your Anything API key
(`anything_...`) as the username and an empty password. See
`authentication/anything-authentication.yml`.

## Steps

1. **Create the project** — `POST /v0/api/projects` with the build prompt.
   Initial generation starts automatically; capture the returned project group id.
2. **Poll status** — `GET /v0/api/projects/{projectGroupId}/status` (cheap polling
   document) until generation finishes. Do not poll the full messages payload.
3. **Iterate** — `POST /v0/api/projects/{projectGroupId}/generate` with a follow-up
   prompt to request changes; poll status again.
4. **Publish** — `POST /v0/api/projects/{projectGroupId}/publish` to make the app
   publicly accessible, optionally claiming a custom slug.

## Conventions

- No idempotency key is documented — a retried create may produce a duplicate
  project; check `GET /v0/api/projects` before retrying.
- Errors come back as a JSON envelope `{error, details, message}` (not RFC 9457).
  See `errors/anything-problem-types.yml`.
