> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## Agent Instructions
> Treat these docs and the Whop MCP server as the source of truth for Whop API behavior. Don't infer endpoint behavior from marketing pages.
> MCP server: connect https://mcp.whop.com/mcp (streamable HTTP, browser sign-in, no API key in the client config) to search these docs and read or change live Whop data. Setup: https://docs.whop.com/developer/guides/ai_and_mcp.md
> Whop CLI: install with `curl -fsSL https://whop.com/install.sh | sh` to scaffold, run, and deploy Whop apps and websites. It acts on production, not the sandbox. Overview: https://docs.whop.com/cli/overview.md Agent mode: https://docs.whop.com/cli/agent-mode.md
> Hosting: Whop serves a site or app from the build `whop apps deploy` uploads, with rollbacks, secrets, and server logs. Guide: https://docs.whop.com/developer/websites/hosting.md
> Plugins: in Claude Code, run `/plugin marketplace add whopio/plugins`, then `/plugin install whop@whop`. Guide: https://docs.whop.com/developer/guides/plugins/claude-code.md Every agent tool: https://docs.whop.com/developer/ai/overview.md
> Environments: build and test in the sandbox (API `https://sandbox-api.whop.com/api/v1`, keys at https://sandbox.whop.com/dashboard/developer, SDK environment `Sandbox`; no real money). Production (API `https://api.whop.com/api/v1`, keys at https://whop.com/dashboard/developer, SDK environment `Production`, the SDK default) moves real money, so use it only at launch. The two share no accounts, keys, products, or webhooks, and their keys look identical: never mix them. Sandbox guide: https://docs.whop.com/developer/guides/sandbox.md Comparison: https://docs.whop.com/developer/quickstart.md
> API: send `Authorization: Bearer <API_KEY>` from your server only, and pin a dated version with the `Api-Version-Date` header. OpenAPI spec: https://api.whop.com/api/v1/openapi.json
> Which API: build on the Current API, whose pages live under https://docs.whop.com/api-reference/ (start at https://docs.whop.com/api-reference/overview.md). Pages under /api-reference/legacy/ are the Legacy API. Use one only when https://docs.whop.com/api-reference/stability.md lists the resource as Legacy-only.

# Verify User Identities with KYC

> Verify your users' identities before they can receive payouts. One API call starts Know Your Customer (KYC) verification, and Whop handles everything else.

## How it works

**1. Start a verification**

Call `POST /api/v1/verifications` with the account you want to verify. You get back a `session_url`.

**2. Send the user to complete KYC**

Redirect your user to the `session_url`. They upload their ID, take a selfie, and verify their identity on a hosted page. You don't build any UI for this.

**3. Listen for the result**

Subscribe to the `identity_profile.updated` webhook — it fires on every change to the profile (approved, rejected, needs action, or a review opening or clearing). The webhook tells you *which* profile changed, not its new state, so call `GET /api/v1/verifications?account_id={biz_ tag}` to read the current status and verified identity data.

## Quick example

```bash cURL theme={null}
curl -X POST "https://api.whop.com/api/v1/verifications?account_id=biz_xxxxxxxxxxxxx" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"kind": "individual"}'
```

```json Response theme={null}
{
  "id": "idpf_xxxxxxxxxxxxx",
  "kind": "individual",
  "status": "pending",
  "session_url": "https://in.sumsub.com/websdk/p/sbx_xxxxxxxxxxxxx",
  "created_at": "2026-06-03T22:15:00Z",
  "updated_at": "2026-06-03T22:15:00Z"
}
```

> [!NOTE]
>
> You need an Account API key with the `identity:write` scope. See [Auth & API keys](https://docs.whop.com/developer/guides/auth-scoping.md) for the credential types and how to scope them. Go to your [dashboard](https://whop.com/dashboard) → Settings → API Keys to create one.

## Pre-fill the identity verification form

Pass identity fields to skip steps for your user. These seed the verification form so they don't have to retype their name and address:

```bash cURL theme={null}
curl -X POST "https://api.whop.com/api/v1/verifications?account_id=biz_xxxxxxxxxxxxx" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "kind": "individual",
    "first_name": "Jane",
    "last_name": "Doe",
    "date_of_birth": "1995-01-15",
    "country": "US",
    "tax_identification_number": "123-45-6789",
    "address": {
      "line1": "123 Main St",
      "city": "San Francisco",
      "state": "CA",
      "postal_code": "94105"
    }
  }'
```

> [!NOTE]
>
> `tax_identification_number` is the person's tax identification number — their SSN for US
> individuals — and is **required when `country` is `US`**: the payout account
> can't be created without it. Send it as-is. It's tokenized in transit, and
> the raw value is never stored on Whop's systems.

> [!WARNING]
>
> Whop can't support users in a [sanctioned country or region](https://docs.whop.com/trust-and-safety/trust-safety-overview/sanctioned-countries.md).
> The request fails with a `400` and the error code `address_country_sanctioned`.

## Reuse a verification you already have

If you already verified this person with Sumsub, send their share token instead of putting them
through KYC a second time. Whop imports the identity your Sumsub account attested and checks it
with Whop's own requirements:

```bash cURL theme={null}
curl -X POST "https://api.whop.com/api/v1/verifications?account_id=biz_xxxxxxxxxxxxx" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"kind": "individual", "share_token": "_act-sbx-jwt-eyJhbGciOiJub25l"}'
```

```json Response theme={null}
{
  "id": "idpf_xxxxxxxxxxxxx",
  "kind": "individual",
  "status": "approved",
  "first_name": "Jane",
  "last_name": "Doe",
  "created_at": "2026-06-03T22:15:00Z",
  "updated_at": "2026-06-03T22:15:00Z"
}
```

**1. Agree on Reusable KYC with Whop**

Sumsub only releases applicant data between clients that have a Reusable KYC agreement. Whop
gives you the client ID to share with once that's in place.

**2. Generate a share token for the applicant**

Call `POST https://api.sumsub.com/resources/accessTokens/shareToken` on your own Sumsub account
with the applicant's ID and Whop's client ID as `forClientId`.

**3. Send it to Whop**

Pass the token as `share_token`. Each token works once.

The response tells you where it landed. `approved` means your verification covered everything Whop
needs and there is nothing left to do. `pending` with a `session_url` means it covered part of it —
send the user to that URL to finish only what's still missing. Whop rejects a token whose data
covers none of the requirements with a `400`.

> [!NOTE]
>
> `share_token` can't be combined with `documents` or `document_type` — each is a complete
> alternative to the hosted flow.

## Two types of verification

| Type | `kind` | What it verifies | When to use |
| - | - | - | - |
| Individual (KYC) | `individual` | A person's identity | User wants to withdraw funds |
| Business (KYB) | `business` | A business entity | Account needs to verify its legal entity |

An account can have one of each — an individual KYC and a business KYB.

## What's next

> [card] [Check status](https://docs.whop.com/developer/verification/retrieve.md)
>
> Check verification status and read verified identity data.

> [card] [Handle RFIs](https://docs.whop.com/developer/verification/rfis.md)
>
> Respond when Whop needs additional information like a bank statement.

> [card] [Update & delete](https://docs.whop.com/developer/verification/manage.md)
>
> Update identity fields or remove a verification.

> [card] [API Reference](https://docs.whop.com/api-reference/verifications/verification)
>
> Full field reference for the verification object.


## Related topics

- [Enable Subaccount Payouts](https://docs.whop.com/developer/platforms/render-payout-portal.md)
- [Users](https://docs.whop.com/api-reference/legacy/users/user.md)
- [Verification](https://docs.whop.com/api-reference/legacy/verifications/verification.md)
