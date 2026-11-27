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

# Cashback Rules API

> A Cashback Rule pays a platform's direct subaccounts back a share of their card spend, funded by the platform.

Each rule sets a rate, an eligibility window, and optional merchant filters, and can be limited to one subaccount.

Use the Cashback Rules API to create, update, and list the rules your platform funds, then pay out cashback on demand from your available USD balance.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Cashback Rules](https://docs.whop.com/api-reference/cashback-rules/list-cashback-rules.md) | GET `/cashback_rules` |
| [Create Cashback Rule](https://docs.whop.com/api-reference/cashback-rules/create-cashback-rule.md) | POST `/cashback_rule` |
| [Pay Out Cashback](https://docs.whop.com/api-reference/cashback-rules/pay-out-cashback.md) | POST `/cashback_rules/payout` |
| [Update Cashback Rule](https://docs.whop.com/api-reference/cashback-rules/update-cashback-rule.md) | PATCH `/cashback_rules/{id}` |

## Pay out cashback

Send an empty object to `POST /cashback_rules/payout` to distribute all eligible cashback. Set `cashback_rule_id` to distribute one rule, `account_id` to pay one subaccount, or `transaction_id` to pay one card transaction. Multiple filters must all match.

```json theme={null}
{
	"cashback_rule_id": "cicbr_7kR2m9Qx4L",
	"account_id": "biz_connectedDemo",
	"transaction_id": "citx_8jF6w3Nv2B"
}
```

Use an `Idempotency-Key` header when requesting a payout. A `202` response with `status: "processing"` confirms that processing was queued and echoes the supplied filters. A `200` response with `status: "failed"` means the queue rejected the request. These statuses describe scheduling, not payment completion. Transaction jobs retry failures automatically. Add USD to the funding wallet if it has insufficient funds. Repeated requests skip payments already recorded, and ledger idempotency prevents duplicate credits.

Response:

```json theme={null}
{
	"status": "processing",
	"cashback_rule_id": "cicbr_7kR2m9Qx4L",
	"account_id": "biz_connectedDemo",
	"transaction_id": "citx_8jF6w3Nv2B"
}
```

## Attributes

**id** `string` *required*

Cashback rule ID, prefixed `cicbr_`.

**created\_at** `string` *required*

When the rule was created, as an ISO 8601 timestamp.

**description** `string | null` *required*

Optional description of the cashback rule.

**discarded\_at** `string | null` *required*

When the rule was discarded, as an ISO 8601 timestamp. Null means it has not
been discarded.

**expires\_at** `string | null` *required*

Exclusive end of the eligibility window, as an ISO 8601 timestamp. Null means
no expiration.

**funding\_account\_id** `string` *required*

Platform account designated to fund cashback, prefixed `biz_`. Derived from
the authenticated credential.

**merchant\_category\_code** `string | null` *required*

Four-digit merchant category code. Null matches any MCC. When both merchant
filters are null, scoped\_account\_id is required.

**merchant\_name** `string | null` *required*

Raw merchant name reported by the card provider. Null matches any merchant
name. When set, matches together with any MCC filter; not a substring or
enriched display-name match.

**rate\_bps** `integer` *required*

Cashback rate in basis points. 100 means 1%, and 10000 means 100%.

**scoped\_account\_id** `string | null` *required*

Subaccount ID, prefixed `biz_`. Null designates all direct subaccounts of the
funding platform.

**starts\_at** `string` *required*

Inclusive start of the rule's eligibility window, as an ISO 8601 timestamp.

**updated\_at** `string` *required*

When the rule was last updated, as an ISO 8601 timestamp.

```json CashbackRule theme={null}
{
	"id": "cicbr_a1B2c3D4e5F6g",
	"merchant_name": "LONE STAR DETAIL SUPPLY",
	"merchant_category_code": "5734",
	"rate_bps": 500,
	"description": "Detailing supplies cashback",
	"funding_account_id": "biz_a1B2c3D4e5F6g",
	"scoped_account_id": "biz_h7I8j9K0l1M2n",
	"starts_at": "2026-10-01T00:00:00.000Z",
	"expires_at": "2026-11-01T00:00:00.000Z",
	"discarded_at": null,
	"created_at": "2026-09-10T08:00:00.000Z",
	"updated_at": "2026-09-10T08:00:00.000Z"
}
```


## Related topics

- [Create Cashback Rule](https://docs.whop.com/api-reference/cashback-rules/create-cashback-rule.md)
- [Update Cashback Rule](https://docs.whop.com/api-reference/cashback-rules/update-cashback-rule.md)
- [List Cashback Rules](https://docs.whop.com/api-reference/cashback-rules/list-cashback-rules.md)
