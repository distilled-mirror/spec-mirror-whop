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

# Exports API

> An Export is a CSV of one of an account's dashboard tables, such as members, payments, or disputes.

Exports are generated in the background, so a new export starts `pending` and becomes `completed` with a download link.

Use the Exports API to start an export, poll it until `download_url` is set, and list an account's past exports. Each table requires its own export scope, and finished files are deleted after 30 days.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Exports](https://docs.whop.com/api-reference/exports/list-exports.md) | GET `/exports` |
| [Retrieve Export](https://docs.whop.com/api-reference/exports/retrieve-export.md) | GET `/exports/{id}` |
| [Create Export](https://docs.whop.com/api-reference/exports/create-export.md) | POST `/exports` |

## Attributes

**id** `string` *required*

Export ID, prefixed `exprt_`.

**created\_at** `string` *required*

When the export was requested, as an ISO 8601 timestamp.

**download\_url** `string | null` *required*

A short-lived link to download the finished file. `null` until `status` is
`completed`, and again once the export has expired.

**expires\_at** `string` *required*

When the file is deleted and the export moves to `expired`, as an ISO 8601
timestamp. Exports are retained for 30 days.

**progress\_percent** `integer | null` *required*

Estimated completion percentage from 0 to 100.

**resource** `string` *required*

The resource that was exported, e.g. `receipts`, `members`, or `payouts`.

Available options: `ad_campaigns`, `ad_groups`, `ads`, `apps`, `audiences`, `bounties`, `bounty_submissions`, `card_transactions`, `checkout_configurations`, `disputes`, `events`, `external_accounts`, `financial-activity`, `payout_methods`, `payouts`, `people`, `plans`, `products`, `receipt_invoices`, `resolution_center_cases`, `shipments`, `social_accounts`, `team_members`, `transfers`, `webhooks`, `members`, `receipts`, `unclaimed_memberships`, `memberships`, `tracking_links`, `promo_codes`, `resolutions`, `entries`, `invoices`, `cancelation_reasons`, `child_companies`, `ledger_lines`, `withdrawal_lines`

**status** `string` *required*

`pending` or `processing` while the file is generated, `completed` when the download is ready, `failed` if it errored, `expired` once the file has been deleted.

Available options: `pending`, `processing`, `completed`, `failed`, `expired`

**updated\_at** `string` *required*

When the export last changed, as an ISO 8601 timestamp.

```json Export theme={null}
{
	"id": "exprt_xxxxxxxxxxxxxx",
	"resource": "members",
	"status": "completed",
	"progress_percent": 100,
	"download_url": "https://whop-assets-example.s3.amazonaws.com/uploads/exports/2026-09-15/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.csv",
	"expires_at": "2026-10-15T14:31:12.000Z",
	"created_at": "2026-09-15T14:30:00.000Z",
	"updated_at": "2026-09-15T14:31:12.000Z"
}
```


## Related topics

- [Export completed](https://docs.whop.com/api-reference/exports/export-completed.md)
- [Export failed](https://docs.whop.com/api-reference/exports/export-failed.md)
- [Retrieve Export](https://docs.whop.com/api-reference/exports/retrieve-export.md)
