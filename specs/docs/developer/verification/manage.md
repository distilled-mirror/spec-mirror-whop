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

# Update or Delete a Verification

> Update identity fields before verification, or remove a verification entirely.

## Update a verification

Update fields on a verification that Whop hasn't approved yet. Use this to correct pre-filled data before the user completes Know Your Customer (KYC) verification.

```bash cURL theme={null}
curl -X PATCH https://api.whop.com/api/v1/verifications/idpf_xxxxxxxxxxxxx \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Janet",
    "last_name": "Smith"
  }'
```

### Updatable fields

| Field | Description |
| - | - |
| `first_name` | Legal first name |
| `last_name` | Legal last name |
| `date_of_birth` | International Organization for Standardization (ISO) date |
| `country` | ISO alpha-2 country code |
| `personal_address` | Residential address object |
| `business_name` | Legal business name |
| `business_address` | Registered business address |
| `business_structure` | Legal entity structure — every value and its countries in [Business structures](https://docs.whop.com/developer/verification/business-structures.md) |

> [!WARNING]
>
> You can't update a verification that's already `approved`. The identity data is locked once verified. Delete it and create a new one if you need to start over.

## Delete a verification

Remove a verification from an account. Whop retains the data but detaches the verification and removes it from the list.

```bash cURL theme={null}
curl -X DELETE https://api.whop.com/api/v1/verifications/idpf_xxxxxxxxxxxxx \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

```json Response theme={null}
{
  "id": "idpf_xxxxxxxxxxxxx",
  "deleted": true
}
```

After deleting, call `POST /api/v1/verifications` to start fresh with a new verification.

> [!NOTE]
>
> Deleting doesn't cancel an in-flight KYC session — it removes the verification link from the account. The session expires on its own.

## Restart a verification

If Whop declined a user's KYC verification or they need to redo it, you don't need to delete and recreate it. Call create with `restart: true`:

```bash cURL theme={null}
curl -X POST https://api.whop.com/api/v1/verifications \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": "biz_xxxxxxxxxxxxx",
    "kind": "individual",
    "restart": true
  }'
```

This abandons the old session and gives you a fresh `session_url`.

## Authentication

All write endpoints require an Account API key with the `identity:write` scope.

### Errors

| Status | Message | When |
| - | - | - |
| `400` | `Cannot update an approved verification.` | Profile is already verified |
| `400` | `at least one field is required` | Empty request body |
| `400` | `Whop can't support users in {place} because it is a sanctioned jurisdiction.` (code `address_country_sanctioned`) | `personal_address` or `business_address` is in a [sanctioned country or region](https://docs.whop.com/trust-and-safety/trust-safety-overview/sanctioned-countries.md) |
| `401` | — | Missing or invalid API key |
| `403` | — | API key lacks `identity:write` scope |
| `404` | `Verification not found` | The `idpf_` ID doesn't exist or isn't visible |


## Related topics

- [Update Verification](https://docs.whop.com/api-reference/verifications/update-verification.md)
- [Verification](https://docs.whop.com/api-reference/legacy/verifications/verification.md)
- [Verifications](https://docs.whop.com/elements/latest/verifications/overview.md)
