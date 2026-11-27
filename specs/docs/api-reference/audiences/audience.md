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

# Audiences API

> An Audience is a reusable group of people to include or exclude when targeting ads.

Build custom audiences from customer lists or Whop People data, and create lookalikes to reach people similar to an existing audience.

Use the Audiences API to create audiences, add people to an uploaded customer list, rename or refilter an audience, and monitor asynchronous processing. Whop People audiences can refresh automatically or keep a snapshot.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Audiences](https://docs.whop.com/api-reference/audiences/list-audiences.md) | GET `/audiences` |
| [Create Audience](https://docs.whop.com/api-reference/audiences/create-audience.md) | POST `/audiences` |
| [Add People](https://docs.whop.com/api-reference/audiences/add-people.md) | POST `/audiences/{id}/add_people` |
| [Update Audience](https://docs.whop.com/api-reference/audiences/update-audience.md) | PATCH `/audiences/{id}` |
| [Delete Audience](https://docs.whop.com/api-reference/audiences/delete-audience.md) | DELETE `/audiences/{id}` |

## Attributes

**id** `string` *required*

Audience ID, prefixed `adaud_`.

**audience\_type** `string` *required*

Whether the audience targets a defined group of people or people similar to an existing audience.

Available options: `custom`, `lookalike`

**auto\_refresh** `boolean` *required*

Whether Whop rebuilds membership from saved People filters twice a day. When
`false`, People audiences keep the members matched at creation. Always `false`
for uploaded lists, lookalikes, and engagement audiences.

**created\_at** `string` *required*

When the audience was created, as an ISO 8601 timestamp.

**error\_message** `string | null` *required*

Why a `pending` audience hasn't started, or why a `partial` or `failed` one
isn't fully built. `null` when there is nothing to report.

**filters** `object | null` *required*

Saved Whop People filters that define membership, using the same keys as `GET
      	/people`. `null` for uploaded lists, engagement audiences, and lookalikes.

**last\_refreshed\_at** `string | null` *required*

When the audience membership was last rebuilt, as an ISO 8601 timestamp.
`null` until the first build completes.

**lookalike\_ratio** `number | null` *required*

For lookalikes: the upper bound of the similarity band as a fraction (0.02 =
top 2%). `null` for custom audiences.

**lookalike\_starting\_ratio** `number | null` *required*

For lookalikes: the lower bound of the similarity band as a fraction. `null`
for custom audiences and first-tier lookalikes.

**matched\_rows** `number` *required*

Members Whop can target with ads. Always 0 for lookalikes and engagement
audiences.

**name** `string` *required*

Audience display name.

**processed\_rows** `number` *required*

Members processed from the source so far. Always 0 for lookalikes and
engagement audiences.

**progress\_percent** `number` *required*

Processing progress from 0 to 100.

**source\_audience\_id** `string | null` *required*

For lookalikes: the audience this lookalike was built from. `null` for custom
audiences.

**source\_type** `string` *required*

Membership source: an uploaded CSV, Whop People filters, or social engagement. Engagement audiences can no longer be created.

Available options: `csv_upload`, `people_filter`, `engagement`

**status** `string` *required*

Processing state. When the audience is `pending`, `partial`, or `failed`, `error_message` says why.

Available options: `pending`, `processing`, `ready`, `partial`, `failed`

**total\_rows** `number` *required*

Members found in the source: CSV rows for an uploaded list, or matching people
for a People audience. Always 0 for lookalikes and engagement audiences.

**updated\_at** `string` *required*

When the audience was last updated, as an ISO 8601 timestamp.

```json Audience theme={null}
{
	"id": "adaud_PastPurchasers123",
	"name": "Past purchasers",
	"status": "ready",
	"audience_type": "custom",
	"source_type": "people_filter",
	"source_audience_id": null,
	"lookalike_ratio": null,
	"lookalike_starting_ratio": null,
	"total_rows": 2500,
	"processed_rows": 2500,
	"matched_rows": 1830,
	"progress_percent": 100,
	"error_message": null,
	"filters": {
		"has_purchased": true,
		"country": "US"
	},
	"auto_refresh": true,
	"last_refreshed_at": "2025-12-01T01:00:00Z",
	"created_at": "2025-12-01T00:00:00Z",
	"updated_at": "2025-12-01T01:00:00Z"
}
```


## Related topics

- [Delete Audience](https://docs.whop.com/api-reference/audiences/delete-audience.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Update Audience](https://docs.whop.com/api-reference/audiences/update-audience.md)
