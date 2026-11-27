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

# API Logs Reference

> An Api Log is a record of a single request made to Whop's API using one of your account's API keys — the programmatic counterpart to the dashboard audit log, which only records actions taken by signed-in team members.

Reads and failed requests are logged too.

Use the Api Logs API to see what your integrations are doing on Whop: the operation, HTTP method and status, outcome, and timing of each request, newest first.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Api Logs](https://docs.whop.com/api-reference/api-logs/list-api-logs.md) | GET `/api_logs` |

## Attributes

**id** `string` *required*

**api\_key\_id** `string | null`

**created\_at** `string` *required*

**duration\_ms** `integer | null`

**http\_method** `string | null`

Available options: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`

**http\_path** `string | null`

**ip\_address** `string | null`

**operation\_name** `string | null`

**resource\_id** `string | null`

**status** `string | null`

Available options: `success`, `error`

**status\_code** `integer | null`

**user\_agent** `string | null`

```json ApiLog theme={null}
{
	"id": "evt_xxxxxxxxxxxxxx_1",
	"api_key_id": "apik_xxxxxxxxxxxxxx",
	"operation_name": "api/v1/products#create",
	"http_method": "POST",
	"http_path": "/api/v1/products",
	"resource_id": "prod_xxxxxxxxxxxxxx",
	"status": "success",
	"status_code": 200,
	"duration_ms": 112,
	"ip_address": "87.106.27.99",
	"user_agent": "whop-sdk-node/1.4.0",
	"created_at": "2026-09-15T14:30:00.000Z"
}
```


## Related topics

- [List API Logs](https://docs.whop.com/api-reference/api-logs/list-api-logs.md)
- [API Keys Reference](https://docs.whop.com/api-reference/api-keys/api-key.md)
- [Stats API Reference](https://docs.whop.com/api-reference/stats/stats.md)
