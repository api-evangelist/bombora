---
name: Build, estimate and activate a Bombora digital audience
description: Compose a custom B2B audience from topics, domains and reference filters, size it, then activate it to a data exchange partner.
api: openapi/bombora-digital-audience-api-openapi.yml
generated: '2026-08-13'
method: generated
source: openapi/bombora-digital-audience-api-openapi.yml
operations:
  - POST /{dataexchange}/estimate
  - POST /{dataexchange}
  - GET /{dataexchange}/{externalId}/{partnerId}
  - PUT /{dataexchange}/{externalId}/{partnerId}
  - PUT /{dataexchange}/{externalId}/{partnerId}/activate
  - PUT /{dataexchange}/{externalId}/{partnerId}/suspend
  - DELETE /{dataexchange}/{externalId}/{partnerId}
---

# Build, estimate and activate a Bombora digital audience

Base URL `https://api.bombora.com/digital-audiences/v1`. Bearer token required — see
`bombora-authenticate.md`. The **Digital Audience API product is manually approved** per app; a
`403` here is an entitlement problem, not a request problem.

Every route is addressed by three path segments: `{dataexchange}` (the destination exchange, see
`components.schemas.DataExchanges`), `{externalId}` (your identifier for the audience) and
`{partnerId}` (the downstream partner).

## Audience composition

The request body is built from:

- `name`, `endDate`, `cpmPrice`, `entityParty`
- `topics` and `domains`
- `surgeId` — sources the audience from a Company Surge signal
- `granularInstallData`, `filterOr`
- `filters` — an object of `{id, values}` pairs, one per attribute family:
  `b2c_interest`, `intent_category`, `ccm_industry`, `ccm_company_size`, `ccm_company_revenue`,
  `ccm_seniority`, `professional_group`, `functional_area`, `install_data`, `country`, `state`

**Resolve every filter `id` and value against the Reference API first** —
`GET https://api.bombora.com/reference/v1/firmographic/industry`, `/demographic/seniority`,
`/geographic/country`, `/install-data/products` and siblings. Unresolvable ids come back as `422`.

## Steps

1. **Size it before you build it.** `POST /{dataexchange}/estimate` returns the projected audience
   size. This is the cheap, non-destructive call — run it while tuning filters. It can return
   `502` if the sizing backend is unavailable; that is retryable with backoff.
2. **Create.** `POST /{dataexchange}` → `201`. Expect `409` if an audience with that
   externalId/partnerId already exists, `422` if the composition is semantically invalid.
3. **Read back.** `GET /{dataexchange}/{externalId}/{partnerId}` and check `status` before doing
   anything else.
4. **Edit.** `PUT /{dataexchange}/{externalId}/{partnerId}` → `204`.
5. **Activate.** `PUT /{dataexchange}/{externalId}/{partnerId}/activate` → `204`. This is the
   commercial action — it puts the audience live with the partner at the stated `cpmPrice`.
   `409` means the audience is not in a state that can be activated.
6. **Suspend.** `PUT /{dataexchange}/{externalId}/{partnerId}/suspend` → `204`. `404` if the
   segment does not exist.
7. **Delete.** `DELETE /{dataexchange}/{externalId}/{partnerId}` → `200`/`204`.

## Rules

- **Activation is not reversible by re-POSTing.** Suspend, do not delete-and-recreate, if you need
  to pause spend — recreating loses the segment's history with the partner.
- There is no idempotency key. A timed-out `POST /{dataexchange}` may still have created the
  audience: `GET` it before retrying.
- The DAB spec declares no `tags` and no `operationId` on any of its seven operations; address
  them by method + path.
- `409` on this API is state-specific and its message is descriptive ("Segment does not have
  prerequired 'Suspended' status"). Read the status via `GET` and act on it; do not parse the
  message.
