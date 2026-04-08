---
doc_area: product/known-issues
last_approver: wilhelm
last_updated: 2026-04-08 15:00:00+00:00
project: auth-migration
sources: []
version: 1
---

## Problem: token-expiry-race-condition

During integration testing, we discovered a race condition when multiple services simultaneously attempt to refresh the same OAuth token. The Clerk token refresh endpoint does not handle concurrent requests idempotently, causing sporadic 401 errors in production under load. We implemented a distributed lock using Redis with a 5-second TTL before calling the refresh endpoint. This resolved the issue but adds latency to the first request after token expiry.
