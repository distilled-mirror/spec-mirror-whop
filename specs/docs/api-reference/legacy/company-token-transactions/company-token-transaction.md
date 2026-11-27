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

# Company Token Transaction

> A token transaction records a credit or debit to a member's token balance within a company, including transfers between members.

```json Example theme={null}
{
	"amount": 6.9,
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"route": "<string>",
		"title": "<string>"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"description": "Reward for completing onboarding",
	"id": "<string>",
	"idempotency_key": "txn_reward_usr_123_2024",
	"linked_transaction_id": "<string>",
	"member": {
		"id": "<string>"
	},
	"transaction_type": "add",
	"user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	}
}
```

**amount** `number` *required*

The token amount for this transaction. Always a positive value regardless of transaction type.

Example: `6.9`

**company** `object` *required*

The company whose token balance this transaction affects.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**route** `string` *required*

The slug/route of the company on the Whop site.

**title** `string` *required*

The written name of the company.

**created\_at** `string\<date-time\>` *required*

The datetime the company token transaction was created.

Example: `2023-12-01T05:00:00.401Z`

**description** `string | null` *required*

Free-text description explaining the reason for this token transaction. Null if no description was provided.

Example: `Reward for completing onboarding`

**id** `string` *required*

The unique identifier for the company token transaction.

**idempotency\_key** `string | null` *required*

A unique key used to prevent duplicate transactions when retrying API requests. Null if no idempotency key was provided.

Example: `txn_reward_usr_123_2024`

**linked\_transaction\_id** `string | null` *required*

The ID of the corresponding transaction on the other side of a transfer. Null
if this is not a transfer transaction.

**member** `object` *required*

The member whose token balance was affected by this transaction.

**child attributes**

**id** `string` *required*

The unique identifier for the company member.

**transaction\_type** `CompanyTokenTransactionTypes` *required*

The direction of this token transaction (add, subtract, or transfer).

Available options: `add`, `subtract`, `transfer`

**user** `object` *required*

The user whose token balance was affected by this transaction.

**child attributes**

**id** `string` *required*

The unique identifier for the user.

Example: `user_xxxxxxxxxxxxx`

**name** `string | null` *required*

The user's display name shown on their public profile.

Example: `Marcus Webb`

**username** `string` *required*

The user's unique username shown on their public profile.

Example: `marcuswebb`


## Related topics

- [Create company token transaction](https://docs.whop.com/api-reference/legacy/company-token-transactions/create-company-token-transaction.md)
- [Retrieve company token transaction](https://docs.whop.com/api-reference/legacy/company-token-transactions/retrieve-company-token-transaction.md)
- [List company token transactions](https://docs.whop.com/api-reference/legacy/company-token-transactions/list-company-token-transactions.md)
