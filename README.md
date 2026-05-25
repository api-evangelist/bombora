# bombora

Bombora — B2B intent data and Company Surge platform, profiled for API Evangelist.

Bombora aggregates anonymous content-consumption signals from a cooperative of 5,000+ B2B publishers and scores accounts against 18,000+ intent topics to identify organizations actively researching specific products, services, and categories. Its developer portal at [developer.bombora.com](https://developer.bombora.com) exposes a catalog of REST APIs covering authentication, intent feeds, reference taxonomies, digital audience activation, webhooks, and Company Surge report orchestration.

## APIs profiled

- **Authentication API** — OAuth 2.0 bearer-token issuance for developer.bombora.com APIs
- **Intent API** — Company Surge intent scores across 18,000+ B2B topics
- **Reference API** — Topic taxonomy and reference metadata
- **Digital Audience Builder (DAB) API** — Compose and activate B2B audiences across data exchanges and DSPs
- **Webhooks API** — Outbound event destinations for Surge and audience updates
- **Company Surge API (v4)** — Partner API at `sentry.bombora.com/v4/Surge` for creating, listing, and retrieving Company Surge reports

Bombora does not publish public OpenAPI specifications — its developer reference docs are gated behind portal login and partner-only Confluence pages — so no `openapi/` artifacts are included in this repo.

## Files

- [apis.yml](apis.yml) — APIs.json 0.20 catalog entry covering the six documented Bombora APIs
