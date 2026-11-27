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

# Review Reports API

> A Review Report is a request from an account's admins for Whop staff to review a customer's review. Staff remove the review if it breaks the guidelines.

Use the Review Reports API to list the reports filed on an account's reviews, narrowed by review, status, or creation date. Check `status` to see whether a report is still pending.

## Endpoints

| Endpoint | Request |
| - | - |
| [List review reports](https://docs.whop.com/api-reference/review-reports/list-review-reports.md) | GET `/review_reports` |

## Attributes

**id** `string` *required*

The report ID, prefixed `rrpt_`.

**created\_at** `string` *required*

When the report was filed, as an ISO 8601 timestamp.

**reason** `string` *required*

The reason the reporter gave for reporting the review.

**review\_id** `string` *required*

The ID of the review that was reported, prefixed `rev_`.

**status** `string` *required*

Where the report stands. `pending` reports are waiting for Whop staff, `accepted` reports led to the review being removed, and `rejected` reports left the review up.

Available options: `pending`, `accepted`, `rejected`

**updated\_at** `string` *required*

When the report last changed, as an ISO 8601 timestamp.

**user\_id** `string | null` *required*

The ID of the user who filed the report, prefixed `user_`. Null if that user
no longer exists.

```json ReviewReport theme={null}
{
	"id": "rrpt_xxxxxxxxxxxxxx",
	"review_id": "rev_xxxxxxxxxxxxxx",
	"user_id": "user_yyyyyyyyyyyyyy",
	"status": "pending",
	"reason": "The reviewer bought a different product",
	"created_at": "2026-09-22T11:30:00.000Z",
	"updated_at": "2026-09-22T11:30:00.000Z"
}
```


## Related topics

- [List Review Reports](https://docs.whop.com/api-reference/review-reports/list-review-reports.md)
- [Reviews API](https://docs.whop.com/api-reference/reviews/review.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
