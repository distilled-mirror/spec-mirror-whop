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

# App

> An app is an integration built on Whop. Apps can serve consumers as experiences within products, or serve companies as business tools.

> [!NOTE]
>
> This resource has a successor in the [Current
> API](https://docs.whop.com/api-reference/legacy/apps/app.md), the versioned API that new integrations
> should build on. This page stays fully supported.

```json Example theme={null}
{
	"api_key": {
		"created_at": "2023-12-01T05:00:00.401Z",
		"id": "<string>",
		"token": "<string>"
	},
	"app_type": "b2b_app",
	"base_url": "https://myapp.example.com",
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"creator": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	},
	"dashboard_path": "/experiences/[experienceId]",
	"default_api_key": {
		"id": "<string>",
		"name": "<string>",
		"obfuscated_secret_key": "<string>",
		"secret_key": "<string>"
	},
	"description": "Book a mobile detailing appointment, choose a package, and pay before your visit.",
	"discover_path": "/experiences/[experienceId]",
	"domain_id": "ab1c2d3e4f5g6h7i8j9k",
	"experience_path": "/experiences/[experienceId]",
	"hosted_url": "https://myapp.whop.site",
	"icon": {
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"id": "app_xxxxxxxxxxxxxx",
	"marketplace_status": "not_available",
	"name": "Courses",
	"oauth_client_type": "public",
	"openapi_path": "/experiences/[experienceId]",
	"origin": "<string>",
	"product_id": "<string>",
	"production_web_build": {
		"checksum": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
		"file_url": "https://cdn.whop.com/builds/abc123.zip",
		"id": "apbu_xxxxxxxxxxxxx",
		"source_url": "https://cdn.whop.com/builds/abc123-source.zip",
		"status": "draft"
	},
	"redirect_uris": ["<string>"],
	"requested_permissions": [
		{
			"is_required": true,
			"justification": "<string>",
			"permission_action": {
				"action": "<string>",
				"name": "<string>"
			}
		}
	],
	"route": "myapp",
	"secrets": {},
	"skills_path": "/experiences/[experienceId]",
	"stats": {
		"dau": 42,
		"mau": 42,
		"time_spent_last24_hours": 42,
		"wau": 42
	},
	"status": "live",
	"verified": true
}
```

**api\_key** `object | null` *required*

The API key used to authenticate requests on behalf of this app. Null if no API key has been generated. Requires the 'developer:manage\_api\_key' permission.

**child attributes**

**created\_at** `string\<date-time\>` *required*

The datetime the private api key was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the private api key.

**token** `string` *required*

This is the API key used to authenticate requests

**app\_type** `AppTypes` *required*

The target audience classification for this app (e.g., 'b2b\_app', 'b2c\_app', 'company\_app', 'component').

Available options: `b2b_app`, `b2c_app`, `company_app`, `component`, `website`

**base\_url** `string | null` *required*

The production base URL where the app is hosted. `null` if no base URL is configured, or if the caller lacks the `developer:basic:read` permission on the app's account.

Example: `https://myapp.example.com`

**company** `object` *required*

The company that owns and publishes this app.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**creator** `object` *required*

The user who created and owns the company that published this app.

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

**dashboard\_path** `string | null` *required*

The URL path template for a specific view of this app, appended to the base domain (e.g., '/experiences/\[experienceId]'). Null if the specified view type is not configured.

Example: `/experiences/[experienceId]`

**default\_api\_key** `object | null` *required*

The app's default API key, used to authenticate requests on behalf of this app. Null if the app has no default key. Requires the 'developer:manage\_api\_key' permission.

**child attributes**

**id** `string` *required*

The unique identifier for the authorized api key.

**name** `string | null` *required*

A user set name to identify an API key

**obfuscated\_secret\_key** `string` *required*

A masked version of the secret key used to authenticate requests. This is so
that the owner can easily identify which key it is without being shown the
full secret.

**secret\_key** `string | null` *required*

The secret key used to authenticate requests. This is only available if the current actor would have been able to create this api key.

**description** `string | null` *required*

A written description of what this app does, displayed on the app store listing page. Null if no description has been set.

Example: `Book a mobile detailing appointment, choose a package, an…`

**discover\_path** `string | null` *required*

The URL path template for a specific view of this app, appended to the base domain (e.g., '/experiences/\[experienceId]'). Null if the specified view type is not configured.

Example: `/experiences/[experienceId]`

**domain\_id** `string` *required*

The unique subdomain identifier for this app's proxied URL on the Whop platform. Forms the URL pattern https\://\{domain\_id}.apps.whop.com.

Example: `ab1c2d3e4f5g6h7i8j9k`

**experience\_path** `string | null` *required*

The URL path template for a specific view of this app, appended to the base domain (e.g., '/experiences/\[experienceId]'). Null if the specified view type is not configured.

Example: `/experiences/[experienceId]`

**hosted\_url** `string | null` *required*

The full canonical URL where this app's hosted web build is served. Null if the app has not claimed a route.

Example: `https://myapp.whop.site`

**icon** `object | null` *required*

The icon image for this app, displayed on the app store, product pages, checkout, and as the default icon for experiences using this app.

**child attributes**

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**id** `string` *required*

The unique identifier for the app.

Example: `app_xxxxxxxxxxxxxx`

**marketplace\_status** `MarketplaceStatuses | null` *required*

The approval status of this app's product listing on the Whop app store. Null if the app has no associated product.

Available options: `not_available`, `pending_review`, `live_marketplace`

**name** `string` *required*

The display name of this app shown on the app store and in experience navigation. Maximum 30 characters.

Example: `Courses`

**oauth\_client\_type** `AppOauthClientTypes` *required*

How this app authenticates when exchanging OAuth authorization and refresh grants.

Available options: `public`, `confidential`

**openapi\_path** `string | null` *required*

The URL path template for a specific view of this app, appended to the base domain (e.g., '/experiences/\[experienceId]'). Null if the specified view type is not configured.

Example: `/experiences/[experienceId]`

**origin** `string | null` *required*

The full origin URL for this app's proxied domain (e.g.,
'[https://myapp.apps.whop.com](https://myapp.apps.whop.com)'). Null if no proxy domain is configured.

**product\_id** `string | null` *required*

**production\_web\_build** `object | null` *required*

The approved app build currently served to users on web. Null if no production build is deployed for web.

**child attributes**

**checksum** `string` *required*

A SHA-256 hash of the uploaded build file, generated by the client and used to verify file integrity.

Example: `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7…`

**file\_url** `string` *required*

A URL to download the app build as a .zip archive.

Example: `https://cdn.whop.com/builds/abc123.zip`

**id** `string` *required*

The unique identifier for the app build.

Example: `apbu_xxxxxxxxxxxxx`

**source\_url** `string | null` *required*

A URL to download the compressed source code archive that produced this build. Null if the build was uploaded without a source archive.

Example: `https://cdn.whop.com/builds/abc123-source.zip`

**status** `AppBuildStatuses` *required*

The current review status of this build.

Available options: `draft`, `pending`, `approved`, `rejected`

**redirect\_uris** `array\<string\>` *required*

The whitelisted OAuth callback URLs that users are redirected to after
authorizing the app.

**requested\_permissions** `array\<object\>` *required*

The list of permissions this app requests when installed, including both required and optional permissions with justifications.

**child attributes**

**is\_required** `boolean` *required*

Whether the action is required for the app to function.

**justification** `string` *required*

The reason for requesting the action.

**permission\_action** `object` *required*

The action that the app will request off of users when a user installs the app.

**child attributes**

**action** `string` *required*

The identifier of the action.

**name** `string` *required*

The human readable name of the action.

**route** `string | null` *required*

The unique subdomain route where this app's hosted web builds are served, such as 'myapp' for myapp.whop.site. Null if the app has not claimed a route.

Example: `myapp`

**secrets** `object | null` *required*

The app's secrets as an object of string values. Encrypted at rest and
injected into the app's hosted server runtime as environment bindings.
Requires the 'developer:update\_app' permission.

**skills\_path** `string | null` *required*

The URL path template for a specific view of this app, appended to the base domain (e.g., '/experiences/\[experienceId]'). Null if the specified view type is not configured.

Example: `/experiences/[experienceId]`

**stats** `object | null` *required*

Aggregate usage statistics for this app, including daily, weekly, and monthly active user counts.

**child attributes**

**dau** `integer` *required*

The number of unique users who have spent time in this app in the last 24 hours. Returns 0 if no usage data is available.

Example: `42`

**mau** `integer` *required*

The number of unique users who have spent time in this app in the last 28 days. Returns 0 if no usage data is available.

Example: `42`

**time\_spent\_last24\_hours** `integer` *required*

The total time, in seconds, that all users have spent in this app over the last 24 hours. Returns 0 if no usage data is available.

Example: `42`

**wau** `integer` *required*

The number of unique users who have spent time in this app in the last 7 days. Returns 0 if no usage data is available.

Example: `42`

**status** `AppStatuses` *required*

The current visibility status of this app on the Whop app store. 'live' means publicly discoverable, 'unlisted' means accessible only via direct link, and 'hidden' means not visible anywhere.

Available options: `live`, `unlisted`, `hidden`

**verified** `boolean` *required*

Whether this app has been verified by Whop. Verified apps are endorsed by Whop
and displayed in the featured apps section of the app store.


## Related topics

- [Apps API](https://docs.whop.com/api-reference/apps/app.md)
- [App Installations](https://docs.whop.com/developer/guides/app-installations.md)
- [App Views](https://docs.whop.com/developer/guides/app-views.md)
