---
name: Resolve Bombora reference attributes
description: Look up the canonical topic, firmographic, demographic, geographic and install-data vocabularies every other Bombora API filters on.
api: openapi/bombora-reference-api-openapi.yml
generated: '2026-08-13'
method: generated
source: openapi/bombora-reference-api-openapi.yml
operations:
  - GET /topics
  - GET /b2c-interest
  - GET /b2b-interest-groups
  - GET /install-data/products
  - GET /firmographic/company-size
  - GET /firmographic/industry
  - GET /firmographic/revenue
  - GET /demographic/functional-area
  - GET /demographic/seniority
  - GET /demographic/professional-group
  - GET /demographic/b2b-personas
  - GET /geographic/country
  - GET /geographic/state
  - GET /geographic/metro-area
---

# Resolve Bombora reference attributes

Base URL `https://api.bombora.com/reference/v1`. Bearer token required — see
`bombora-authenticate.md`. This product is auto-approved, so it is usually the first Bombora API
an integration can call successfully, and the right one to smoke-test credentials against.

**Do this before every write to any other Bombora API.** Signal definitions, account-list filters
and digital-audience filters all name attribute ids from these fourteen endpoints. An id that is
not in the current vocabulary comes back as `422` from the API that consumed it, not from here.

## The fourteen vocabularies

| Domain | Endpoint |
|---|---|
| Intent | `GET /topics` — the 18,000+ B2B intent topics |
| Intent | `GET /b2c-interest` |
| Intent | `GET /b2b-interest-groups` |
| Intent / Demographic | `GET /demographic/b2b-personas` |
| Install Data | `GET /install-data/products` |
| Firmographic | `GET /firmographic/company-size` |
| Firmographic | `GET /firmographic/industry` |
| Firmographic | `GET /firmographic/revenue` |
| Demographic | `GET /demographic/functional-area` |
| Demographic | `GET /demographic/seniority` |
| Demographic | `GET /demographic/professional-group` |
| Geographic | `GET /geographic/country` |
| Geographic | `GET /geographic/state` |
| Geographic | `GET /geographic/metro-area` |

## Filtering

Every list endpoint accepts the same query parameters where relevant: `id`, `name`,
`description`, `category`, `theme`, `s` (free-text search) and `maxTopics` (caps topic
expansion). Use `s` to find a topic by phrase, then carry the returned **id** — never the display
name — into the consuming API.

## Rules

- **Cache, but expire.** The taxonomy changes: the only breaking change Bombora ever published in
  its release notes was a renamed industry filter value ("Finance > Venture Capital Private
  Equity" → "Finance > Venture Capital Private Equity & Fund Raising", 2020-03-23). Refresh
  periodically and key on id, not label.
- The API Change Policy declares that **adding new enum/text values to responses is non-breaking
  and may ship without notice** — your code must tolerate unknown values in these lists.
- These endpoints are **not paginated** — no `limit`/`pageToken` on the Reference API. Use the
  filter parameters to narrow instead.
- Only three statuses are declared: `200`, `400` (syntactically invalid), `422` (semantically
  invalid). Notably `401`/`403` are not declared here even though the API is bearer-protected —
  handle them anyway.
