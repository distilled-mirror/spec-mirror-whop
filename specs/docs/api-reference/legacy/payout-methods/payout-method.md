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

# Payout Method

> A configured payout destination where a user receives earned funds, such as a bank account or digital wallet.

```json Example theme={null}
{
	"account_reference": "****1234",
	"company": {
		"id": "biz_xxxxxxxxxxxxxx"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "USD",
	"destination": {
		"category": "crypto",
		"country_code": "<string>",
		"name": "<string>"
	},
	"id": "potk_xxxxxxxxxxxxx",
	"institution_name": "Frost Bank",
	"is_default": true,
	"nickname": "Ops checking"
}
```

**account\_reference** `string | null` *required*

A masked identifier for the payout destination, such as the last four digits of a bank account or an email address. Null if no reference is available.

Example: `****1234`

**company** `object | null` *required*

The company associated with this payout destination. Null if not linked to a specific company.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**created\_at** `string\<date-time\>` *required*

The datetime the payout token was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `string` *required*

The three-letter ISO currency code that payouts are delivered in for this destination.

Example: `USD`

**destination** `object | null` *required*

The payout destination configuration linked to this token. Null if not yet configured.

**child attributes**

**category** `PayoutDestinationCategory` *required*

The category of the payout destination

Available options: `crypto`, `rtp`, `next_day_bank`, `bank_wire`, `digital_wallet`, `unknown`

**country\_code** `string` *required*

The country code of the payout destination

**name** `string` *required*

The name of the payer associated with the payout destination

**id** `string` *required*

The unique identifier for the payout token.

Example: `potk_xxxxxxxxxxxxx`

**institution\_name** `string | null` *required*

The name of the bank or financial institution receiving payouts. Null if not applicable or not provided.

Example: `Frost Bank`

**is\_default** `boolean` *required*

Whether this is the default payout destination for the associated payout
account.

**nickname** `string | null` *required*

A user-defined label to help identify this payout destination. Not sent to the provider. Null if no nickname has been set.

Example: `Ops checking`


## Related topics

- [Payout method created](https://docs.whop.com/api-reference/payouts/payout-method-created.md)
- [Retrieve payout method](https://docs.whop.com/api-reference/legacy/payout-methods/retrieve-payout-method.md)
- [List payout methods](https://docs.whop.com/api-reference/legacy/payout-methods/list-payout-methods.md)
