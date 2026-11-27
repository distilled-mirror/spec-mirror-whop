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

# Authorized User

> A user who belongs to a company's team with access determined by their assigned role.

```json Example theme={null}
{
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"id": "ausr_xxxxxxxxxxxxx",
	"role": "owner",
	"user": {
		"email": "john.doe@example.com",
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	}
}
```

**company** `object` *required*

The company this authorized user has access to.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**id** `string` *required*

The team member's ID. It may start with `ausr_` or `mber_`; treat it as opaque.

Example: `ausr_xxxxxxxxxxxxx`

**role** `AuthorizedUserRoles` *required*

The permission role assigned to this authorized user within the company.

Available options: `owner`, `admin`, `partner`, `sales_manager`, `moderator`, `advertiser`, `app_manager`, `support`, `manager`, `workforce`, `custom`

**user** `object` *required*

The user account linked to this authorized user record.

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

- [Create authorized user](https://docs.whop.com/api-reference/legacy/authorized-users/create-authorized-user.md)
- [Delete authorized user](https://docs.whop.com/api-reference/legacy/authorized-users/delete-authorized-user.md)
- [List authorized users](https://docs.whop.com/api-reference/legacy/authorized-users/list-authorized-users.md)
