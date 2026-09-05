---
name: Deploy a Git repository on Deployxa
description: Create a Deployxa project from a Git repository and trigger its first production deployment using the published REST API.
api: openapi/deployxa-openapi-original.json
operations: [createProject, createDeployment]
generated: '2026-09-05'
method: generated
---

# Deploy a Git repository on Deployxa

## Before you start

- Base URL: `https://deployxa.com/api/v1` (from the spec's `servers[]`).
- Auth: send `Authorization: Bearer <key>` — either an API key created at
  https://deployxa.com/dashboard/api-keys, or an OAuth 2.0 access token from the
  authorization-code + PKCE flow documented at https://deployxa.com/auth.md.
- **Both operations are paid**: each carries an `x-payment-info` extension
  declaring a $1.00 USD Stripe charge per call (project provisioning /
  deployment execution). There is no documented idempotency key and no
  documented reversal or cancel operation, so do not retry blindly and confirm
  intent before calling.

## Steps

1. **Create the project** — `POST /projects` (`createProject`). Body (required):
   `name` and `repositoryUrl` (a Git repository URI); optional `branch`.
   A `201` returns a `Project` with `id`, `name`, `status`.
2. **Trigger the deployment** — `POST /deployments` (`createDeployment`). Body:
   `projectId` (required, from step 1) and optional `branch`. A `201` returns a
   `Deployment` with `id` and `status`.
3. **Handle errors** — `400` and `500` return the shared envelope
   `{code, message, details?}` (see `errors/deployxa-problem-types.yml`).
   Deprecated operations announce `Deprecation` and `Sunset` headers — check
   for them and plan migration when present.

## Notes

- The contract publishes no read/list/status-polling operations; deployment
  health is surfaced in the dashboard and via the auth-gated MCP server at
  `https://mcp.deployxa.com/mcp` (scopes `deployments:read`, `logs:read`).
