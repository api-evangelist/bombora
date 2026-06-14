# Bombora GraphQL Schema

Bombora is the leading provider of cooperative-sourced B2B intent data. Its flagship product, Company Surge, aggregates anonymous content-consumption signals from more than 5,000 B2B publishers and scores accounts against 18,000+ B2B intent topics to surface organizations actively researching specific products, services, and categories.

This conceptual GraphQL schema models the core entities and relationships across Bombora's developer APIs: the Intent API, Reference API, Digital Audience Builder API, Company Surge API (v4), Webhooks API, and Authentication API.

## Schema Coverage

The schema covers 60+ named types organized into the following functional domains:

### Company and Firmographic Data
- `Company` — core account record keyed on domain
- `CompanyDetails` — full firmographic profile
- `CompanyDomain` — domain and subdomain identifiers
- `CompanySize` — employee count ranges and headcount estimates
- `CompanyIndustry` — SIC/NAICS industry classification
- `CompanyRevenue` — annual revenue ranges
- `CompanyType` — public/private/nonprofit/government classification
- `CompanyLocation` — headquarters and branch geography

### Intent and Surge Scoring
- `IntentData` — aggregate intent record for a company+topic pair
- `IntentDetails` — detailed per-topic intent breakdown
- `IntentScore` — normalized 0–100 intent score
- `SurgeScore` — week-over-week surge score
- `SurgeDetails` — metadata about a surge score event
- `CompanySurge` — Company Surge record combining account + topics
- `SurgeRanking` — percentile ranking within topic/industry peer groups
- `SurgeCompositeScore` — multi-topic composite surge signal
- `WeeklyScore` — rolling 7-day aggregated score
- `MonthlyScore` — rolling 30-day aggregated score

### Topics and Taxonomy
- `Topic` — Bombora intent topic (one of 18,000+)
- `TopicDetails` — description and metadata for a topic
- `TopicCategory` — high-level category grouping
- `TopicCluster` — thematic cluster of related topics
- `SubTopic` — granular sub-topic beneath a parent topic
- `ContentTopic` — topic as inferred from content consumption
- `Taxonomy` — root taxonomy object for the Bombora reference taxonomy
- `TaxonomyDetails` — version metadata and structure information

### Buyer Journey and Stages
- `BuyerJourney` — full buyer journey model for an account
- `BuyingStage` — TOFU/MOFU/BOFU stage classification
- `StageDetails` — evidence and signals underlying a stage assignment

### Intent Signals and Trends
- `IntentSignal` — individual content-consumption signal event
- `SignalType` — category of signal (article read, whitepaper download, etc.)
- `SignalStrength` — strength classification of a signal
- `IntentTrend` — directional trend in intent over time
- `TrendDetails` — trend metadata including velocity and acceleration

### Contacts and B2B Identity
- `ContactDetails` — business contact details
- `B2BContact` — resolved B2B individual identity
- `ContactRole` — functional role (buyer, influencer, champion, etc.)
- `ContactSeniority` — seniority level classification
- `ContactIntent` — intent signals attributed to a contact
- `CompanyIntentProfile` — intent snapshot for an entire account

### Audiences and Activation
- `Segment` — audience segment definition
- `AudienceDetails` — audience composition and reach metadata

### CRM and Platform Integrations
- `CRMMatch` — match record linking Bombora data to a CRM object
- `SalesforceLead` — Salesforce lead/contact enrichment record
- `HubSpotContact` — HubSpot contact enrichment record
- `IntegrationDetails` — integration configuration and sync state

### Predictions and Models
- `Prediction` — ML-generated account-level prediction
- `PredictionModel` — model metadata and version information

### API Operations and Authentication
- `APIKey` — API key credential record
- `Token` — OAuth 2.0 access token
- `BulkQuery` — bulk data export query record
- `QueryResults` — paginated results for bulk queries

## Root Operations

### Queries
- `company(domain: String!)` — fetch a company by domain
- `companySurge(domain: String!, topicIds: [ID!])` — fetch surge scores for an account
- `topic(id: ID!)` — fetch a topic by ID
- `topics(clusterId: ID, categoryId: ID, search: String)` — list/search topics
- `taxonomy` — fetch the current Bombora taxonomy
- `intentFeed(startDate: Date!, endDate: Date!, topicIds: [ID!]!)` — pull weekly intent feed
- `intentTrend(domain: String!, topicId: ID!, weeks: Int)` — trend over time
- `segment(id: ID!)` — fetch an audience segment
- `bulkQuery(id: ID!)` — retrieve bulk query status and results
- `token` — inspect current OAuth token metadata

### Mutations
- `createSurgeReport(input: SurgeReportInput!)` — create a Company Surge report (v4)
- `createSegment(input: SegmentInput!)` — compose a new audience segment
- `activateAudience(segmentId: ID!, partnerId: ID!, dataExchange: String!)` — push audience to DSP
- `registerWebhookDestination(input: WebhookDestinationInput!)` — register a webhook endpoint
- `updateWebhookDestination(id: ID!, input: WebhookDestinationInput!)` — update a webhook
- `createBulkQuery(input: BulkQueryInput!)` — submit a bulk intent data export

## Authentication

All operations require a Bearer access token obtained from the Bombora OAuth 2.0 token endpoint at `https://developer.bombora.com/docs/authentication-api/1/overview`. Company Surge API (v4) on `sentry.bombora.com` uses HTTP Basic auth with base64-encoded `username:password` credentials.

## Reference

- Developer Portal: https://developer.bombora.com
- Intent API: https://developer.bombora.com/docs/intent-api/1/overview
- Reference API: https://developer.bombora.com/docs/reference-api/1/overview
- Digital Audience API: https://developer.bombora.com/docs/digital-audience-api/1/overview
- Company Surge API: https://bombora-partners.atlassian.net/wiki/spaces/DOC/pages/20381698/Surge+API
- Webhooks API: https://developer.bombora.com/docs/webhooks-api/1/overview
