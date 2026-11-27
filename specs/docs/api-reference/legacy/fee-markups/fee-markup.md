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

# Fee Markup

> A fee markup configuration that defines additional charges applied to transactions for a platform's connected accounts.

```json Example theme={null}
{
	"created_at": "2023-12-01T05:00:00.401Z",
	"fee_type": "crypto_withdrawal_markup",
	"fixed_fee_usd": 6.9,
	"id": "<string>",
	"notes": "Approved by finance team on 2024-01-15",
	"percentage_fee": 6.9,
	"updated_at": "2023-12-01T05:00:00.401Z"
}
```

**created\_at** `string\<date-time\>` *required*

The datetime the fee markup was created.

Example: `2023-12-01T05:00:00.401Z`

**fee\_type** `FeeMarkupTypes` *required*

The category of fee this markup applies to.

Available options: `crypto_withdrawal_markup`, `rtp_withdrawal_markup`, `next_day_bank_withdrawal_markup`, `bank_wire_withdrawal_markup`, `digital_wallet_withdrawal_markup`, `transfer_markup`, `crypto_deposit_markup`, `bank_deposit_markup`, `crypto_swap_markup`, `payment_markup`

**fixed\_fee\_usd** `number | null` *required*

A flat fee charged per transaction, in USD. Ranges from 0 to 50. Null if no fixed fee is configured.

Example: `6.9`

**id** `string` *required*

The unique identifier for the fee markup.

**notes** `string | null` *required*

Internal notes about this fee markup, visible only to administrators. Null if no notes have been added.

Example: `Approved by finance team on 2024-01-15`

**percentage\_fee** `number | null` *required*

A percentage-based fee charged per transaction. Ranges from 0 to 25. Null if no percentage fee is configured.

Example: `6.9`

**updated\_at** `string\<date-time\>` *required*

The datetime the fee markup was last updated.

Example: `2023-12-01T05:00:00.401Z`


## Related topics

- [Accounts API](https://docs.whop.com/api-reference/accounts/account.md)
- [Create fee markup](https://docs.whop.com/api-reference/legacy/fee-markups/create-fee-markup.md)
- [Delete fee markup](https://docs.whop.com/api-reference/legacy/fee-markups/delete-fee-markup.md)
