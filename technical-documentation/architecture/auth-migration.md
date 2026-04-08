---
doc_area: technical-documentation/architecture
last_approver: wilhelm
last_updated: 2026-04-08 15:00:00+00:00
project: auth-migration
sources: []
version: 1
---

## Decision: clerk-vs-auth0

We evaluated Clerk and Auth0 for the authentication migration. We chose Clerk because of its excellent developer experience, competitive pricing at our scale of 7-10 developers, and native support for multi-tenant token management. Auth0 has stronger enterprise SSO integrations, but those are not required for our current phase. Clerk's SDK is significantly simpler to integrate with our existing Next.js stack.

## Architecture: clerk-middleware-stack

The authentication middleware sits between the API gateway and all downstream services. Clerk's JWT verification happens at the gateway level using the Clerk SDK verify function. Valid tokens are passed downstream with user context injected into request headers. Token refresh is handled transparently by the middleware without the downstream service being aware. We added a circuit breaker to prevent auth failures from cascading into service outages.
