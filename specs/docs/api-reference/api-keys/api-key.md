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

# API Keys Reference

> An API Key is a programmatic credential owned by an account or app.

Each key carries its own permissions policy — explicit permission statements or an inherited system role — and can be restricted with an expiration date and an IP allowlist.

Use the API Keys API to list an account or app's keys, create a key (the full secret is returned once, on creation), inspect a key's effective grants, update its name or restrictions, rotate its secret, and revoke it. These endpoints require a user session — they cannot be called with an API key.

## Version pinning

Each API key stores an `api_version_date`. Requests authenticated with the key use that version when they omit the `Api-Version-Date` header. An explicit header takes precedence. New keys default to the latest released version, while legacy keys without a stored pin use `2025-01-01`.

## Endpoints

| Endpoint | Request |
| - | - |
| [List API Keys](https://docs.whop.com/api-reference/api-keys/list-api-keys.md) | GET `/api_keys` |
| [Retrieve API Key](https://docs.whop.com/api-reference/api-keys/retrieve-api-key.md) | GET `/api_keys/{id}` |
| [List the Permission Catalog](https://docs.whop.com/api-reference/api-keys/list-the-permission-catalog.md) | GET `/api_keys/permissions` |
| [Create API Key](https://docs.whop.com/api-reference/api-keys/create-api-key.md) | POST `/api_keys` |
| [Rotate API Key](https://docs.whop.com/api-reference/api-keys/rotate-api-key.md) | POST `/api_keys/{id}/rotate` |
| [Update API Key](https://docs.whop.com/api-reference/api-keys/update-api-key.md) | PATCH `/api_keys/{id}` |
| [Delete API Key](https://docs.whop.com/api-reference/api-keys/delete-api-key.md) | DELETE `/api_keys/{id}` |


## Related topics

- [Stats API Reference](https://docs.whop.com/api-reference/stats/stats.md)
- [Create API Key](https://docs.whop.com/api-reference/api-keys/create-api-key.md)
- [Update API Key](https://docs.whop.com/api-reference/api-keys/update-api-key.md)
