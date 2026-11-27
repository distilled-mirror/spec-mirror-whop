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

# Member

> A member represents a user's relationship with a company on Whop, including their access level, status, and spending history.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/members/member.md), the versioned API that new
> integrations should build on. This page stays fully supported.

```json Example theme={null}
{
	"access_level": "no_access",
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"route": "<string>",
		"title": "<string>"
	},
	"company_token_balance": 6.9,
	"created_at": "2023-12-01T05:00:00.401Z",
	"id": "<string>",
	"joined_at": "2023-12-01T05:00:00.401Z",
	"most_recent_action": "canceling",
	"most_recent_action_at": "2023-12-01T05:00:00.401Z",
	"phone": "<string>",
	"status": "drafted",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"usd_total_spent": 6.9,
	"user": {
		"email": "<string>",
		"id": "<string>",
		"name": "<string>",
		"username": "<string>"
	}
}
```

**access\_level** `AccessLevel` *required*

The member's content access level. `admin` means their team role grants administrative content access, `customer` means they hold a valid product membership, and `no_access` means they cannot access company content.

Available options: `no_access`, `admin`, `customer`

**company** `object` *required*

The company for the member.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**route** `string` *required*

The slug/route of the company on the Whop site.

**title** `string` *required*

The written name of the company.

**company\_token\_balance** `number` *required*

The member's token balance for this company. Computed live from the ledger, not from a cache.

Example: `6.9`

**created\_at** `string\<date-time\>` *required*

The datetime the company member was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the company member.

**joined\_at** `string\<date-time\>` *required*

When the member joined the company

Example: `2023-12-01T05:00:00.401Z`

**most\_recent\_action** `MemberMostRecentActions | null` *required*

The most recent action the member has taken.

Available options: `canceling`, `churned`, `finished_split_pay`, `paused`, `paid_subscriber`, `paid_once`, `expiring`, `joined`, `drafted`, `left`, `trialing`, `pending_entry`, `renewing`, `past_due`

**most\_recent\_action\_at** `string\<date-time\> | null` *required*

The time for the most recent action, if applicable.

Example: `2023-12-01T05:00:00.401Z`

**phone** `string | null` *required*

The phone number for the member, if available.

**status** `MemberStatuses` *required*

The status of the member

Available options: `drafted`, `joined`, `left`

**updated\_at** `string\<date-time\>` *required*

The datetime the company member was last updated.

Example: `2023-12-01T05:00:00.401Z`

**usd\_total\_spent** `number` *required*

How much money this customer has spent on the company's products and plans

Example: `6.9`

**user** `object | null` *required*

The user for this member, if any.

**child attributes**

**email** `string | null` *required*

The digital mailing address of the user.

**id** `string` *required*

The unique identifier for the company member user.

**name** `string | null` *required*

The user's full name.

**username** `string` *required*

The whop username.


## Related topics

- [Members API](https://docs.whop.com/api-reference/members/member.md)
- [Member created](https://docs.whop.com/api-reference/legacy/members/member-created.md)
- [Member updated](https://docs.whop.com/api-reference/legacy/members/member-updated.md)
