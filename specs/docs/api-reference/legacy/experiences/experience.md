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

# Experience

> An experience is a feature or content module within a product, such as a chat, course, or custom app.

```json Example theme={null}
{
	"app": {
		"icon": {
			"url": "https://media.whop.com/abc123/optimized.jpg"
		},
		"id": "app_xxxxxxxxxxxxxx",
		"name": "Courses"
	},
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"route": "shine-time-auto-detailing",
		"title": "Shine Time Auto Detailing"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"id": "exp_xxxxxxxxxxxxxx",
	"image": {
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"is_public": true,
	"name": "Detailing Tips Chat",
	"order": "123.45",
	"products": [
		{
			"id": "prod_xxxxxxxxxxxxx",
			"route": "ceramic-coating-package",
			"title": "Ceramic Coating Package"
		}
	]
}
```

**app** `object` *required*

The app that powers this experience, defining its interface and behavior.

**child attributes**

**icon** `object | null` *required*

The icon image for this app, displayed on the app store, product pages, checkout, and as the default icon for experiences using this app.

**child attributes**

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**id** `string` *required*

The unique identifier for the app.

Example: `app_xxxxxxxxxxxxxx`

**name** `string` *required*

The display name of this app shown on the app store and in experience navigation. Maximum 30 characters.

Example: `Courses`

**company** `object` *required*

The company that owns this experience.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**route** `string` *required*

URL slug for the account's store page. For example, `shine-time-auto-detailing` in whop.com/shine-time-auto-detailing.

Example: `shine-time-auto-detailing`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**created\_at** `string\<date-time\>` *required*

The datetime the experience was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the experience.

Example: `exp_xxxxxxxxxxxxxx`

**image** `object | null` *required*

The custom logo image for this experience. Null if no custom logo has been uploaded.

**child attributes**

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**is\_public** `boolean` *required*

Whether this experience is publicly visible to all users, including those
without a membership.

**name** `string` *required*

The display name of this experience shown to users in the product navigation. Maximum 255 characters.

Example: `Detailing Tips Chat`

**order** `string | null` *required*

The sort position of this experience within its section. Lower values appear first. Null if no position has been set.

Example: `123.45`

**products** `array\<object\>` *required*

The list of products this experience is attached to, which determines which customers have access. Empty if the experience is only visible to authorized company team members.

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**route** `string` *required*

URL slug in the product's public link, e.g. `ceramic-coating-package` in whop.com/company/ceramic-coating-package.

Example: `ceramic-coating-package`

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`


## Related topics

- [Create experience](https://docs.whop.com/api-reference/legacy/experiences/create-experience.md)
- [Retrieve experience](https://docs.whop.com/api-reference/legacy/experiences/retrieve-experience.md)
- [Attach experience](https://docs.whop.com/api-reference/legacy/experiences/attach-experience.md)
