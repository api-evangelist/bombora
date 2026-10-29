# Bombora

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Bombora is a New York-based B2B data company that provides intent data and identity solutions used by marketing, sales, and ad-tech teams to identify in-market accounts and personalize outreach. Its flagship product, Company Surge, aggregates anonymous content-consumption signals from a cooperative of more than 5,000 B2B publishers and scores accounts against 18,000+ B2B intent topics to surface organizations actively researching specific products, services, and categories. Beyond Company Surge, Bombora offers Identity and Enrichment for first-party visitor resolution, Digital Audiences for activation across DSPs and walled gardens, Campaign Measurement for B2B attribution, and an Insights Suite that combines intent, visitor, and engagement signals. Bombora runs an Apigee Integrated Developer Portal at developer.bombora.com that publishes six OpenAPI 3.0 documents anonymously — Intent, Reference, Account List, Digital Audience Builder, Webhooks, and Authentication — covering 54 operations on api.bombora.com, all secured with OAuth 2.0 client credentials exchanged for a bearer JWT at POST /oauth/token. A separate legacy partner Company Surge API (v4) runs on sentry.bombora.com with HTTP Basic auth and a documented 60 calls-per-minute-per-endpoint limit. Credentials are not self-service: the portal is SAML-only and restricted to existing Bombora customers, the Intent and Digital Audience products require manual per-application approval, and no pricing is published — commercial access is sold via annual contracts. Bombora integrates broadly with the B2B revenue stack, including Salesforce, HubSpot, LinkedIn, 6sense, Demandbase, Apollo, ZoomInfo, Dun and Bradstreet, Snowflake, and Adobe Experience Platform, and is profiled here for its role as the dominant supplier of cooperative-sourced B2B intent data.

**Areas:** Intent Data · B2B Intent · Company Surge · Account Based Marketing · Sales Intelligence · Marketing Intelligence · Identity Resolution · Audience Activation · Data Cooperative · Webhook · Reference Data · AdTech

## Bombora on APIs.io

Everything profiled for Bombora is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/bombora/](https://apis.io/providers/bombora/)** — or go straight to one of the APIs or properties below.

### APIs (17)

- [Bombora Intent API](https://apis.io/apis/bombora/bombora-intent-api/) — Streams Bombora's Company Surge intent signals — account-level scores against 18,000+ B2B intent topics — derived from the Bombora Data Cooperative.…
- [Bombora Company Surge API (v4)](https://apis.io/apis/bombora/bombora-company-surge-api/) — Partner API for orchestrating Company Surge reports — create reports with topic, geography, blacklist, and AutoGen options (POST /v4/Surge/Create), list…
- [Bombora Accounts API](https://apis.io/apis/bombora/bombora-accounts-api/) — The Accounts API from Bombora — 4 operation(s) for accounts.
- [Bombora Activate API](https://apis.io/apis/bombora/bombora-activate-api/) — The Activate API from Bombora — 1 operation(s) for activate.
- [Bombora Data API](https://apis.io/apis/bombora/bombora-data-api/) — The Data API from Bombora — 1 operation(s) for data.
- [Bombora Demographic API](https://apis.io/apis/bombora/bombora-demographic-api/) — The Demographic API from Bombora — 4 operation(s) for demographic.
- [Bombora Destinations API](https://apis.io/apis/bombora/bombora-destinations-api/) — The Destinations API from Bombora — 4 operation(s) for destinations.
- [Bombora Digital Audience Builder (DAB) API](https://apis.io/apis/bombora/bombora-digital-audience-builder-dab-api-api/) — The Digital Audience Builder (DAB) API API from Bombora — 2 operation(s) for digital audience builder (dab) api.
- [Bombora Estimate API](https://apis.io/apis/bombora/bombora-estimate-api/) — The Estimate API from Bombora — 1 operation(s) for estimate.
- [Bombora Events API](https://apis.io/apis/bombora/bombora-events-api/) — The Events API from Bombora — 2 operation(s) for events.
- [Bombora Firmographic API](https://apis.io/apis/bombora/bombora-firmographic-api/) — The Firmographic API from Bombora — 3 operation(s) for firmographic.
- [Bombora Geographic API](https://apis.io/apis/bombora/bombora-geographic-api/) — The Geographic API from Bombora — 3 operation(s) for geographic.
- [Bombora Install Data API](https://apis.io/apis/bombora/bombora-install-data-api/) — The Install Data API from Bombora — 1 operation(s) for install data.
- [Bombora Signal Definition API](https://apis.io/apis/bombora/bombora-signaldefinition-api/) — The SignalDefinition API from Bombora — 5 operation(s) for signaldefinition.
- [Bombora Suspend API](https://apis.io/apis/bombora/bombora-suspend-api/) — The Suspend API from Bombora — 1 operation(s) for suspend.
- [Bombora OAUTH API](https://apis.io/apis/bombora/bombora-oauth-api/) — The Oauth API from Bombora — 1 operation(s) for oauth.
- [Bombora account lists API](https://apis.io/apis/bombora/bombora-account-lists-api/) — The account lists API from Bombora — 2 operation(s) for account lists.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Bombora Webhooks](https://apis.io/asyncapis/bombora/bombora-webhooks/)

#### GraphQL (1)

GraphQL schemas.

- [Moved to /providers/bombora/](https://apis.io/graphqls/bombora/bombora-graphql/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [bombora-wp-mcp](https://apis.io/servers/bombora/bombora-wp-mcp/)

#### Collections (6)

Postman and Open Collection request collections.

- [Account List API](https://apis.io/collections/bombora/open-bombora-account-list-api/)
- [Authentication API](https://apis.io/collections/bombora/open-bombora-authentication-api/)
- [Digital Audience Builder (DAB) API](https://apis.io/collections/bombora/open-bombora-digital-audience-api/)
- [Intent API](https://apis.io/collections/bombora/open-bombora-intent-api/)
- [Reference API](https://apis.io/collections/bombora/open-bombora-reference-api/)
- [Webhooks API](https://apis.io/collections/bombora/open-bombora-webhooks-api/)

#### Overlays (6)

OpenAPI overlays, including conversational phrasing for each operation.

- [Bombora Account List Api Overlay](https://apis.io/overlays/bombora/bombora-account-list-api-overlay/)
- [Bombora Authentication Api Overlay](https://apis.io/overlays/bombora/bombora-authentication-api-overlay/)
- [Bombora Digital Audience Api Overlay](https://apis.io/overlays/bombora/bombora-digital-audience-api-overlay/)
- [Bombora Intent Api Overlay](https://apis.io/overlays/bombora/bombora-intent-api-overlay/)
- [Bombora Reference Api Overlay](https://apis.io/overlays/bombora/bombora-reference-api-overlay/)
- [Bombora Webhooks Api Overlay](https://apis.io/overlays/bombora/bombora-webhooks-api-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Bombora Authentication](https://apis.io/security/bombora/bombora-authentication/)
- [Bombora Domain Security](https://apis.io/security/bombora/bombora-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Bombora Plans Pricing](https://apis.io/plans/bombora/bombora-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Bombora Rate Limits](https://apis.io/rate-limits/bombora/bombora-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://bombora.com)
- [Documentation](https://developer.bombora.com/docs/intent-api/1/overview)
- [Getting started](https://developer.bombora.com/get-started)
- [API reference](https://developer.bombora.com/apis)
- [Sign up](https://developer.bombora.com/login)
- [Support](https://bombora.com/customer-support-forms/)
- [Blog](https://bombora.com/feed/)
- [Terms of service](https://bombora.com/terms)
- [Privacy policy](https://bombora.com/privacy-policy)
- [GitHub](https://github.com/bomboradata)

## API Evangelist rating

**Kin Score 44.7** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 80.0 |
| Contract quality | 60.3 |
| Contract governance | 4.5 |
| Operational transparency | 47.4 |
| Developer ergonomics | 30.4 |
| Access clarity | 21.1 |

**Agent readiness 42.7** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | platform (partial credit) |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/bombora/](https://apis.io/providers/bombora/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Bombora: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/bombora/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
