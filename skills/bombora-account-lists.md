---
name: Build and maintain a Bombora account list
description: Create manual, derived, Surge-sourced or visitor-insights account lists and manage their account membership.
api: openapi/bombora-account-list-api-openapi.yml
generated: '2026-08-13'
method: generated
source: openapi/bombora-account-list-api-openapi.yml
operations:
  - GET /account-list
  - POST /account-list
  - GET /account-list/{accountListId}
  - PUT /account-list/{accountListId}
  - DELETE /account-list/{accountListId}
  - updateAccounts
  - POST /account-list/{accountListId}/accounts
  - DELETE /account-list/{accountListId}/accounts
  - GET /account-list/{accountListId}/accounts/domains
  - GET /account-list/{accountListId}/accounts/search
  - POST /account-list/{accountListId}/accounts/search
  - POST /account-list/{accountListId}/delete-accounts
---

# Build and maintain a Bombora account list

Base URL `https://api.bombora.com/account-list/v1`. Bearer token required — see
`bombora-authenticate.md`. Accounts are keyed on **company domain**, not on a Bombora id.

## Pick the list type first

`POST /account-list` takes an `AccountListDefinition` whose `specification` shape depends on
`type`:

- **Manual** — `ManualAccountListSpecification`. You supply the accounts.
- **Derived** — `DerivedAccountListSpecification` with `parentId` (another account list) plus a
  `filter` built from the predicate grammar (`Eq`, `Neq`, `In`, `Nin`, `Gt`, `Gte`, `Lt`, `Lte`,
  `And`, `Or`). Segments an existing list.
- **Surge** — `SurgeAccountListSpecification` with a `signalDefinitionId`, `topics`, `attributes`
  and `filters`. Sourced from Company Surge intent; create the signal definition first
  (`bombora-signal-definition-and-intent.md`).
- **VisitorInsights** — `VisitorInsightsAccountListSpecification` with `attributes` and `filters`.

Every attribute named in a `filter` leaf must exist in the Reference API — resolve it with
`GET /firmographic/*`, `GET /demographic/*`, `GET /geographic/*` before you send it, or you will
get a `422`.

## Steps

1. `GET /account-list` — list existing lists (`limit` + `pageToken` cursor paging). Check for a
   duplicate before creating one; there is no idempotency key.
2. `POST /account-list` — create. `200` means created synchronously; **`202` means the list is
   being built asynchronously** and `Metadata.pendingUpdate` will be true until it settles.
3. `GET /account-list/{accountListId}` — read back. `AccountListSummary` carries `id`,
   `parentId`, `type`, `name`, `description`, `accountTotal`, `accountsLastModified`,
   `pendingUpdate`, `createdDate`, `modifiedDate`.
4. **Membership (manual lists only):**
   - `POST /account-list/{accountListId}/accounts` — `operationId: updateAccounts`, the only
     operationId Bombora declares anywhere in its six specs. Adds or updates accounts. May return
     `202`.
   - `POST /account-list/{accountListId}/delete-accounts` — remove specific accounts.
   - `DELETE /account-list/{accountListId}/accounts` — empties the list but keeps the list itself.
     Documented as idempotent and **valid only for Manual lists**.
5. **Read membership:**
   - `GET /account-list/{accountListId}/accounts/domains` — just the domains.
   - `GET /account-list/{accountListId}/accounts/search` — account records with attributes and
     company firmographics. Use `POST .../accounts/search` when the query is too large for a query
     string.
6. `PUT /account-list/{accountListId}` — update definition/metadata.
   `DELETE /account-list/{accountListId}` — delete. Expect `409` if the list is in use, e.g. it is
   the `parentId` of a derived list.

## Rules

- Wait out `pendingUpdate` before reading membership; a list mid-build returns partial counts.
- `409` across this API always means "conflict with the current state of the resource (pending
  update or currently in use)" — it is a wait-or-unlink condition, never a retry-immediately one.
- Subscribe to the `AccountListAccountsUpdated` webhook event rather than polling `accountTotal`.
- Errors are `{"message": "..."}` with no code. Branch on status.
