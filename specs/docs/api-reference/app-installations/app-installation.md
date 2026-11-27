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

# App Installations API

> An App Installation represents one install of your app on one account.

An account installs an app once, however many experiences it adds, so an installation holds state that belongs to the account rather than to one experience, such as a sync cursor or the ID of the account it's linked to in another system.

Use the App Installations API to [list the accounts your app is installed on](https://docs.whop.com/api-reference/app-installations/list-app-installations.md), [retrieve one](https://docs.whop.com/api-reference/app-installations/retrieve-app-installation.md), and [update](https://docs.whop.com/api-reference/app-installations/update-app-installation.md) the `metadata` and `external_id` your app keeps on it. An account can also list the apps installed on it, and a developer every installation of their app. Only the app and its developer see `metadata` and `external_id`, and only the app can change them.

## Endpoints

| Endpoint | Request |
| - | - |
| [List App Installations](https://docs.whop.com/api-reference/app-installations/list-app-installations.md) | GET `/app_installations` |
| [Retrieve App Installation](https://docs.whop.com/api-reference/app-installations/retrieve-app-installation.md) | GET `/app_installations/{id}` |
| [Update App Installation](https://docs.whop.com/api-reference/app-installations/update-app-installation.md) | PATCH `/app_installations/{id}` |

## Attributes

**id** `string` *required*

App installation ID, prefixed `aag_`.

**account** `object` *required*

Account the app is installed on.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **title** `string` *required*
>
> Account display name.

**app** `object` *required*

App that is installed.

> [accordion] Properties
>
> **id** `string` *required*
>
> App ID, prefixed `app_`.
>
> **developer** `object` *required*
>
> Account that develops the app.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Account ID, prefixed `biz_`.
> >
> > **title** `string` *required*
> >
> > Account display name.
>
> **icon** `object` *required*
>
> App icon. Falls back to the default app icon when none is uploaded.
>
> > [accordion] Properties
> >
> > **url** `string` *required*
> >
> > Icon image URL. Always present — the default app icon when none is uploaded.
>
> **name** `string` *required*
>
> App display name.

**created\_at** `string` *required*

When the app was first installed on the account, as an ISO 8601 timestamp.
Installing the app again later keeps the original time.

**external\_id** `string | null` *required*

Your app's own identifier for this installation, such as the ID of the linked
account in another system, or `null` when none is set. At most 255 characters
and unique across your app's installations, so you can find an installation
with `GET /app_installations?external_id=`; setting one another installation
already has returns a `409`. Only the app and its developer see it; for the
account the app is installed on it is always `null`.

**metadata** `object | null` *required*

String keys and values your app stores about this installation, such as a sync
cursor or the ID of the linked account in another system. Empty until your app
sets a key, and kept if the account removes the app and installs it again. At
most 50 keys; keys are at most 100 characters and values at most 500. Only the
app can change it. Only the app and its developer see it; for the account the
app is installed on it is always `null`.

**requesting\_reauthorization** `boolean` *required*

Whether the app now asks for permissions the account hasn't approved yet.

**updated\_at** `string` *required*

When the installation last changed, such as a `metadata` update or the account
changing the app's permissions, as an ISO 8601 timestamp.

```json AppInstallation theme={null}
{
	"id": "aag_T4k8M2p6W9r3Qx",
	"app": {
		"id": "app_Q7m3V9k2X5t8Lp",
		"name": "Shine Time Booking",
		"icon": {
			"url": "https://assets.whop.com/apps/shine-time-booking/icon.png"
		},
		"developer": {
			"id": "biz_W3r7N1k5P8q2Hd",
			"title": "Shine Time Auto Detailing"
		}
	},
	"account": {
		"id": "biz_J8n2R5p9T1w4Kx",
		"title": "Shine Time Auto Detailing"
	},
	"external_id": "shine-time-austin",
	"metadata": {
		"last_synced_at": "2026-10-16T21:00:00Z",
		"quickbooks_realm_id": "9341452712"
	},
	"requesting_reauthorization": false,
	"created_at": "2026-09-02T15:30:00.000Z",
	"updated_at": "2026-10-16T21:00:04.000Z"
}
```


## Related topics

- [App Installations](https://docs.whop.com/developer/guides/app-installations.md)
- [List App Installations](https://docs.whop.com/api-reference/app-installations/list-app-installations.md)
- [App installation deleted](https://docs.whop.com/api-reference/app-installations/app-installation-deleted.md)
