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

# Card Transaction

> A card transaction record.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/cards/list-card-transactions.md). This page stays fully
> supported.

```json Example theme={null}
{
	"authorization_method": "ecommerce",
	"card_id": "<string>",
	"cashback_usd_amount": 6.9,
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "USD",
	"declined_reason": "insufficient_funds",
	"id": "<string>",
	"international": true,
	"local_amount": 6.9,
	"memo": "Team dinner",
	"merchant_category": "Software",
	"merchant_category_code": "5734",
	"merchant_icon_url": "https://logo.clearbit.com/example.com",
	"merchant_name": "Lone Star Detail Supply",
	"posted_at": "2023-12-01T05:00:00.401Z",
	"status": "pending",
	"transaction_type": "spend",
	"usd_amount": 6.9
}
```

**authorization\_method** `string | null` *required*

How the card was presented or authenticated for the purchase.

Example: `ecommerce`

**card\_id** `string` *required*

**cashback\_usd\_amount** `number | null` *required*

The cashback reward amount earned on this transaction, in USD.

Example: `6.9`

**created\_at** `string\<date-time\>` *required*

The datetime the card transaction was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `string | null` *required*

The ISO 4217 currency code for the transaction amount.

Example: `USD`

**declined\_reason** `string | null` *required*

The issuer-provided reason the transaction was declined.

Example: `insufficient_funds`

**id** `string` *required*

The unique identifier for the card transaction.

**international** `boolean` *required*

Whether the transaction was made with a merchant outside the card's home
country.

**local\_amount** `number | null` *required*

The transaction amount in the merchant's local currency before conversion.

Example: `6.9`

**memo** `string | null` *required*

A user-provided note attached to the transaction.

Example: `Team dinner`

**merchant\_category** `string | null` *required*

The enriched or raw category label for the merchant.

Example: `Software`

**merchant\_category\_code** `string | null` *required*

The four-digit ISO 18245 merchant category code (MCC).

Example: `5734`

**merchant\_icon\_url** `string | null` *required*

A URL to the enriched merchant logo image.

Example: `https://logo.clearbit.com/example.com`

**merchant\_name** `string | null` *required*

The enriched or raw name of the merchant where the purchase was made.

Example: `Lone Star Detail Supply`

**posted\_at** `string\<date-time\> | null` *required*

When the transaction was settled by the card network.

Example: `2023-12-01T05:00:00.401Z`

**status** `CardIssuingTransactionStatus` *required*

The current lifecycle status of the transaction.

Available options: `pending`, `completed`, `reversed`, `declined`

**transaction\_type** `string` *required*

The type of transaction.

Example: `spend`

**usd\_amount** `number | null` *required*

The transaction amount in USD.

Example: `6.9`


## Related topics

- [List Card Transactions](https://docs.whop.com/api-reference/cards/list-card-transactions.md)
- [Retrieve Card Transaction](https://docs.whop.com/api-reference/cards/retrieve-card-transaction.md)
- [Card transaction reversed](https://docs.whop.com/api-reference/legacy/card-transactions/card-transaction-reversed.md)
