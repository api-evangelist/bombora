---
name: Authenticate against the Bombora APIs
description: Exchange a portal-issued ClientId/ClientSecret for a bearer token and call any Bombora v1 API with it.
api: openapi/bombora-authentication-api-openapi.yml
generated: '2026-08-13'
method: generated
source: openapi/bombora-authentication-api-openapi.yml, https://developer.bombora.com/get-started
operations:
  - POST /oauth/token
---

# Authenticate against the Bombora APIs

Every Bombora product API (Intent, Reference, Account List, Digital Audience, Webhooks) is
protected by the same scheme: `bearerAuth`, an `http`/`bearer` JWT sent in the `Authorization`
header. You get that JWT from one place.

## Before you start — this is not self-service

You cannot obtain credentials programmatically. A human must:

1. Sign in at `https://developer.bombora.com` (the only option is **LOGIN WITH SAML**; you must
   already be a Bombora user).
2. Have Bombora Support associate the account with the organisation's developer team.
3. Open **My Apps → team → app** and read **ClientId** (key) and **ClientSecret** (secret).
4. Confirm the **Authentication API** product is enabled on that app — otherwise `/oauth/token`
   fails — and that the product you intend to call is enabled too. **Intent API** and **Digital
   Audience API** require *manual* approval by Bombora; Reference, Account List and Webhooks are
   auto-approved but still need a provisioned team.

If any of the above is missing, stop and route the user to
`https://bombora.com/customer-support-forms/`. There is no way to self-serve past it.

## Steps

1. `POST /oauth/token` on the Authentication API — base `https://api.bombora.com`.
   Present the ClientId and ClientSecret as the client credentials. Read the access token off the
   response.
2. Send every subsequent call to the product host with
   `Authorization: Bearer <access_token>`:
   - Intent API — `https://api.bombora.com/intent/v1`
   - Reference API — `https://api.bombora.com/reference/v1`
   - Account List API — `https://api.bombora.com/account-list/v1`
   - Digital Audience API — `https://api.bombora.com/digital-audiences/v1`
   - Webhooks API — `https://api.bombora.com/webhooks/v1`
3. Cache the token and re-mint on `401`. Do not decode it — the API Change Policy declares all
   tokens opaque and variable in length, and says they may change without notice.

## Failure handling

- `401` — token missing, expired or invalid. Re-mint once, then stop.
- `403` — the token is valid but the app does not have that API product enabled. This is an
  entitlement problem, not a retry problem. Do not loop.
- Error bodies are `application/json` with a single field: `{"message": "..."}`. There is no
  error code. Branch on the HTTP status, never on the message text — Bombora reserves the right
  to change message text without notice.

## Do not confuse these two OAuth surfaces

`https://bombora.com/.well-known/oauth-authorization-server` describes the WordPress MCP server
on Bombora's marketing site (scope `mcp`). It has nothing to do with `api.bombora.com`. Tokens
from it will not authenticate a product API call.

The legacy partner **Company Surge API v4** at `https://sentry.bombora.com/v4/Surge` uses HTTP
Basic (base64 `username:password`) instead — a different credential entirely.
