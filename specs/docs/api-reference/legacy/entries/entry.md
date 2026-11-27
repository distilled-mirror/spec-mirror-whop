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

# Entry

> An entry represents a user's signup for a waitlisted plan.

```json Example theme={null}
{
	"created_at": "2023-12-01T05:00:00.401Z",
	"custom_field_responses": [
		{
			"answer": "<string>",
			"id": "cfrp_xxxxxxxxxxxxx",
			"question": "<string>"
		}
	],
	"id": "entry_xxxxxxxxxxxx",
	"plan": {
		"id": "plan_xxxxxxxxxxxxx"
	},
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	},
	"status": "drafted",
	"user": {
		"email": "john.doe@example.com",
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	}
}
```

**created\_at** `string\<date-time\> | null` *required*

The datetime the entry was created.

Example: `2023-12-01T05:00:00.401Z`

**custom\_field\_responses** `array\<object\> | null` *required*

The list of responses collected from the user when submitting their waitlist entry.

**child attributes**

**answer** `string` *required*

The response a user gave to the specific question or field.

**id** `string` *required*

The unique identifier for the custom field response.

Example: `cfrp_xxxxxxxxxxxxx`

**question** `string` *required*

The question asked by the custom field

**id** `string` *required*

The unique identifier for the entry.

Example: `entry_xxxxxxxxxxxx`

**plan** `object | null` *required*

The waitlisted plan that this entry is a signup for.

**child attributes**

**id** `string` *required*

The unique identifier for the plan.

Example: `plan_xxxxxxxxxxxxx`

**product** `object | null` *required*

The product associated with this entry's waitlisted plan. Null if the plan is not tied to a product.

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`

**status** `EntryStatus` *required*

The current status of the waitlist entry (e.g., drafted, pending, approved, denied).

Available options: `drafted`, `pending`, `approved`, `denied`, `any`

**user** `object` *required*

The user who submitted this waitlist entry.

**child attributes**

**email** `string | null` *required*

The user's email address. Requires the member:email:read permission to access. Null if not authorized.

Example: `john.doe@example.com`

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

- [Entry approved](https://docs.whop.com/api-reference/legacy/entries/entry-approved.md)
- [Entry created](https://docs.whop.com/api-reference/legacy/entries/entry-created.md)
- [Entry deleted](https://docs.whop.com/api-reference/legacy/entries/entry-deleted.md)
