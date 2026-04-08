---
doc_area: product/best-practices
last_approver: wilhelm
last_updated: 2026-04-08 15:00:00+00:00
project: auth-migration
sources: []
version: 1
---

## Good-to-know: clerk-webhook-retry-behavior

Clerk webhooks use an exponential backoff retry policy with up to 5 attempts over 24 hours. If your webhook endpoint returns a non-2xx response, Clerk will retry. This means your webhook handler must be idempotent. We discovered this when a transient database error during user creation caused duplicate user records on retry. Always check for existing records before creating new ones in webhook handlers.
