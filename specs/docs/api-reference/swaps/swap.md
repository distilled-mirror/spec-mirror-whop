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

# Swaps API

> A Swap converts value between supported tokens, chains, or wallet destinations, or between fiat currencies at the mid-market rate, for an account or user.

A swap quote previews the expected output, fees, and approval requirements before you create the swap.

Use the Swaps API to quote a conversion, create the swap, list recent swaps, and retrieve a swap's status until it completes.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Swaps](https://docs.whop.com/api-reference/swaps/list-swaps.md) | GET `/swaps` |
| [Retrieve Swap](https://docs.whop.com/api-reference/swaps/retrieve-swap.md) | GET `/swaps/{id}` |
| [Create Swap](https://docs.whop.com/api-reference/swaps/create-swap.md) | POST `/swaps` |
| [Create Swap Quote](https://docs.whop.com/api-reference/swaps/create-swap-quote.md) | POST `/swaps/quote` |

## Attributes

**id** `string` *required*

Swap ID.

**account\_id** `string` *required*

Account that owns the swap: a business ID prefixed `biz_`, or the user ID for
a personal account.

**amount** `string | null` *required*

The from\_token amount the swap was created to spend, as a decimal string. A
swap created with `all` reports the wallet balance it read. Null for a fiat
pair.

**error** `string | null`

Latest error returned for a failed swap.

**object** `string` *required*

**status** `string` *required*

Current swap status. `complete` and `failed` are terminal.

Available options: `queued`, `working`, `complete`, `failed`

**tx\_hashes** `string[]` *required*

On-chain transaction hashes produced by the swap.

**user\_id** `string | null` *required*

User whose personal account owns the swap, prefixed `user_`. Null for a
business account.

```json Swap theme={null}
{
	"id": "swap_xxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"amount": "0.052341",
	"error": null,
	"object": "swap",
	"status": "complete",
	"tx_hashes": ["0x1234567890abcdef"],
	"user_id": null
}
```


## Related topics

- [ConvertElement](https://docs.whop.com/elements/latest/wallet/convert.md)
- [List Swaps](https://docs.whop.com/api-reference/swaps/list-swaps.md)
- [Retrieve Swap](https://docs.whop.com/api-reference/swaps/retrieve-swap.md)
