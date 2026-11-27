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

# Check Verification Status

> List an account's KYC and KYB verifications or retrieve one by ID, and see what each status means.

## List verifications for an account

Pass the account's `biz_` tag as the `account_id` query parameter to get all verifications for that account. An account has at most two — one individual Know Your Customer (KYC) verification and one Know Your Business (KYB) verification.

```bash cURL theme={null}
curl "https://api.whop.com/api/v1/verifications?account_id=biz_xxxxxxxxxxxxx" \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

```json Response theme={null}
{
  "data": [
    {
      "id": "idpf_xxxxxxxxxxxxx",
      "kind": "individual",
      "status": "approved",
      "first_name": "Jane",
      "last_name": "Doe",
      "date_of_birth": "1995-01-15",
      "country": "US",
      "address": {
        "line1": "123 Main St",
        "city": "San Francisco",
        "state": "CA",
        "postal_code": "94105"
      },
      "requested_information": [],
      "created_at": "2026-06-03T22:15:00Z",
      "updated_at": "2026-06-04T10:00:00Z"
    }
  ]
}
```

## Retrieve a single verification

Pass an `idpf_` tag in the path to read one profile directly.

```bash cURL theme={null}
curl https://api.whop.com/api/v1/verifications/idpf_xxxxxxxxxxxxx \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

The response is a single verification object (the same shape as an entry in the list's `data` array).

## Statuses

| Status | What it means | What to do |
| - | - | - |
| `pending` | User hasn't completed KYC yet | Send them to `session_url` |
| `approved` | Identity verified | User can transact |
| `rejected` | KYC declined | Create a new verification with `restart: true` |
| `action_required` | Whop needs more information | Check the `requested_information` array — see [Handle RFIs](https://docs.whop.com/developer/verification/rfis.md) |
| `not_started` | Profile exists, no session started | Call create to start |

> [!NOTE]
>
> `session_url` only appears when `status` is `pending` — it means the user still needs to complete KYC. Once approved or rejected, the URL is gone.

## Response fields

Fields depend on `kind`. Individual verifications return personal fields, business verifications return account fields.

### Individual (`kind: "individual"`)

| Field | Type | Description |
| - | - | - |
| `first_name` | `string` | Verified legal first name |
| `last_name` | `string` | Verified legal last name |
| `date_of_birth` | `string` | International Organization for Standardization (ISO) date (e.g. `1995-01-15`) |
| `country` | `string` | ISO country code |
| `address` | `object` | Verified residential address |

### Business (`kind: "business"`)

| Field | Type | Description |
| - | - | - |
| `business_name` | `string` | Verified legal entity name |
| `business_structure` | `string` | Legal entity structure, such as `private_corporation` or `sole_proprietorship` — see [Business structures](https://docs.whop.com/developer/verification/business-structures.md) |
| `registration_number` | `string` | Verified number the business was registered under in its place of incorporation, such as a state file number in the US |
| `country` | `string` | ISO country code |
| `address` | `object` | Registered business address |

## Authentication

Requires an Account API key with the `identity:read` scope.

### Errors

| Status | Message | When |
| - | - | - |
| `400` | `account_id is required` | No `biz_` tag provided |
| `401` | — | Missing or invalid API key |
| `403` | — | API key lacks `identity:read` scope |
| `404` | `Account not found` | The `biz_` tag doesn't exist or you don't have access |


## Related topics

- [Verify User Identities with KYC](https://docs.whop.com/developer/verification/overview.md)
- [Check Entitlements](https://docs.whop.com/developer/guides/ios/check-entitlements.md)
- [Verification](https://docs.whop.com/api-reference/legacy/verifications/verification.md)
