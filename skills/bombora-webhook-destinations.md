---
name: Receive Bombora events over webhooks
description: Register a webhook destination, sign it with an HMAC secret, subscribe to Bombora event types and read delivery statistics.
api: openapi/bombora-webhooks-api-openapi.yml
generated: '2026-08-13'
method: generated
source: openapi/bombora-webhooks-api-openapi.yml
operations:
  - POST /destination
  - GET /destinations
  - GET /destination/{destinationId}
  - PUT /destination/{destinationId}
  - PUT /destination/{destinationId}/auth
  - GET /destination/{destinationId}/events
  - GET /destination/{destinationId}/event/{eventType}
  - PUT /destination/{destinationId}/event/{eventType}
  - DELETE /destination/{destinationId}/event/{eventType}
  - DELETE /destination/{destinationId}
---

# Receive Bombora events over webhooks

Base URL `https://api.bombora.com/webhooks/v1`. Bearer token required — see
`bombora-authenticate.md`. This is the only push surface Bombora publishes; there is no AsyncAPI
document and no streaming API.

Two resources: **Destinations** (where events go) and **Events** (per-destination subscriptions).

## Steps

1. **Create the destination.** `POST /destination` with `name`, `description` and `address` — a
   fully qualified HTTPS URL Bombora will POST to. Optionally attach static `headers` (e.g.
   `X-Source: Bombora`) that Bombora will echo on every delivery so your receiver can route them.
2. **Set the signing secret immediately.**
   `PUT /destination/{destinationId}/auth` with `{"secret": "<your secret>"}`. Bombora then
   computes an HMAC-SHA256 over the UTF-8 body keyed by that secret and sends it in the
   **`X-Bombora-Signature-256`** header. Verify it on every inbound request before trusting the
   payload.
   The secret is **write-only** — `GET /destination/{destinationId}` explicitly excludes the
   `auth` object, so store your copy. Rotating means PUTting a new one.
3. **List what you can subscribe to.** `GET /destination/{destinationId}/events` returns each
   `eventType` with an `enabled` flag. The types Bombora documents are:
   - `SignalDefinitionCreated`
   - `SignalDefinitionUpdated`
   - `SignalDefinitionDeleted`
   - `AccountListAccountsUpdated`
   Treat this list as authoritative at runtime — `eventType` is an open string with no enum in the
   spec, so Bombora may serve more than the four above.
4. **Subscribe.** `PUT /destination/{destinationId}/event/{eventType}` with
   `{"enabled": true}` and optional per-event `headers` (e.g. `X-Message-Type: signal-is-ready`).
5. **Monitor delivery.** `GET /destination/{destinationId}/event/{eventType}` returns the event
   config merged with stats: `successful`, `failed`, `lastError`, `lastErrorAction`,
   `lastErrorTimestamp`. This is the only delivery observability Bombora offers — poll it after a
   change rather than assuming success.
6. **Unsubscribe.** `DELETE /destination/{destinationId}/event/{eventType}`.
7. **Delete the destination.** `DELETE /destination/{destinationId}` — **fails with `409` unless
   every event on it is disabled first.** Disable or delete all subscriptions, then delete the
   destination.

## Rules

- **No payload schema is published for any event.** The specs describe subscription management
  only, not the delivered message body. Write your receiver defensively: verify the signature,
  log the raw body, and do not assume field names.
- No retry or backoff policy is published. Use `failed`/`lastError` on the subscription to detect
  drops; make your receiver idempotent since redelivery behaviour is undocumented.
- Prefer webhooks over polling for signal-definition readiness and account-list membership
  changes — both are covered by the four event types above.
- Errors are `{"message": "..."}` with no code. `409` on delete means "events must be first
  disabled".
