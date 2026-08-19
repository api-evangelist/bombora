---
name: Define a Bombora intent signal and pull its data
description: Create a signal definition from Reference API topics, then retrieve Company Surge intent data against it.
api: openapi/bombora-intent-api-openapi.yml
generated: '2026-08-13'
method: generated
source: openapi/bombora-intent-api-openapi.yml, openapi/bombora-reference-api-openapi.yml
operations:
  - GET /topics
  - POST /signal-definition
  - GET /signal-definitions
  - GET /signal-definition/{signalDefinitionId}
  - PUT /signal-definition/{signalDefinitionId}
  - GET /signal-definition/{signalDefinitionId}/metadata
  - PUT /signal-definition/{signalDefinitionId}/metadata
  - GET /signal-definition/{signalDefinitionId}/product-definition
  - PUT /signal-definition/{signalDefinitionId}/product-definition
  - GET /data
  - POST /data
  - DELETE /signal-definition/{signalDefinitionId}
---

# Define a Bombora intent signal and pull its data

Base URLs: Intent API `https://api.bombora.com/intent/v1`, Reference API
`https://api.bombora.com/reference/v1`. Both require `Authorization: Bearer <token>` — see
`bombora-authenticate.md`. The **Intent API product is manually approved** per app; if you get
`403` here, entitlement is the cause.

A *signal definition* is the saved question ("who is researching these topics"). Intent data is
the answer. You always create the definition first.

## Steps

1. **Resolve topics.** `GET /topics` on the Reference API. Filter with `id`, `name`,
   `description`, `category`, `theme`, `s` (free-text search) and `maxTopics`. Never hard-code a
   topic string — the Reference API is the canonical vocabulary and topic names change.
   Firmographic, demographic and geographic attribute values used in filters come from the same
   API: `GET /firmographic/industry`, `/firmographic/company-size`, `/firmographic/revenue`,
   `/demographic/seniority`, `/demographic/functional-area`, `/demographic/professional-group`,
   `/demographic/b2b-personas`, `/geographic/country`, `/geographic/state`,
   `/geographic/metro-area`, `/install-data/products`, `/b2c-interest`, `/b2b-interest-groups`.
2. **Create the definition.** `POST /signal-definition` with the topic set and a `metadata`
   object (`name`, `description`). This returns **202** — creation is asynchronous. Do not treat
   the 202 as "the signal is ready".
3. **Poll for readiness.** `GET /signal-definitions` to list, or
   `GET /signal-definition/{signalDefinitionId}` for the one you created. Prefer subscribing to
   the `SignalDefinitionCreated` webhook event instead of polling — see
   `bombora-webhook-destinations.md`.
4. **Optionally attach a product definition.**
   `PUT /signal-definition/{signalDefinitionId}/product-definition` carries `keywords`, `urls`
   and `domain` so Bombora can tune the signal to a specific product.
   `PUT /signal-definition/{signalDefinitionId}/metadata` updates name/description alone.
5. **Retrieve intent data.** `GET /data` for simple retrievals. Use `POST /data` when the request
   is too large for a query string — the filter grammar (`Eq`, `Neq`, `In`, `Nin`, `Gt`, `Gte`,
   `Lt`, `Lte`, composed with `And`/`Or`) goes in the body.
6. **Page through.** Both `/data` and `/signal-definitions` accept `limit` and `pageToken`.
   `pageToken` is opaque and variable-length by published policy — pass it back verbatim, never
   parse or persist it as an identifier.
7. **Clean up.** `DELETE /signal-definition/{signalDefinitionId}` when the definition is no
   longer needed. It returns `409` if the definition is in use — for example by a Surge account
   list that references it via `signalDefinitionId`.

## Rules

- **No idempotency key exists.** `POST /signal-definition` is not replay-safe. If a POST times
  out, `GET /signal-definitions` and check before retrying, or you will create a duplicate.
- **No rate-limit headers.** Bombora publishes no `RateLimit-*` or `Retry-After` headers and
  declares no `429` on any Intent API operation. Self-throttle; back off on any `429` you observe.
- `400` = syntactically invalid request. `422` = syntactically valid but semantically wrong
  (validation error) — usually an attribute or topic id that does not exist in the Reference API.
  `409` = conflict with the current state (pending update, or in use).
