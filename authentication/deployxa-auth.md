# auth.md

Deployxa agent authentication uses OAuth 2.0 bearer tokens. Agents should register through the documented authorization flow and send credentials in the HTTP Authorization header.

## Registration

- Audience: automated agents integrating with the Deployxa API.
- Registration endpoint: `/api/auth/extension/start`.
- Method: OAuth authorization-code flow with PKCE.
- Start: POST `/api/auth/extension/start` with `state` and `codeChallenge`; follow the returned `authorizationUrl` for user consent.
- Exchange: POST the returned authorization code, original state, and PKCE verifier to `/api/auth/extension/exchange`.
- Credential use: `Authorization: Bearer <access-token>`.

See `/.well-known/oauth-protected-resource` and `/.well-known/oauth-authorization-server` for machine-readable metadata.
