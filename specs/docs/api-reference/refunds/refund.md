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

# Refunds API

> A Refund is one full or partial reversal of a payment, issued with Refund Payment. It records how much moved, through which provider, and whether it succeeded.

A Refund is one full or partial reversal of a payment, issued with [Refund Payment](https://docs.whop.com/api-reference/payments/refund-payment.md). It records how much moved, through which provider, and whether it succeeded.

Use the Refunds API to list the refunds on a payment or across an account, and to retrieve a single refund.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Refunds](https://docs.whop.com/api-reference/refunds/list-refunds.md) | GET `/refunds` |
| [Retrieve Refund](https://docs.whop.com/api-reference/refunds/retrieve-refund.md) | GET `/refunds/{id}` |

## Attributes

**id** `string` *required*

Refund ID, prefixed `rf_`.

**account\_id** `string | null` *required*

The account that issued the refund, prefixed `biz_`.

**amount** `object | null` *required*

The refunded amount as it settled, in the payment's settlement currency, so pages of refunds net against the payment's `refunded_amount`. Converted at the rate in force when the refund was issued, not the payment's original rate. Null only when no exchange rate is recorded for a legacy multi-currency payment.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**created\_at** `string` *required*

When the refund was requested, as an ISO 8601 timestamp.

**failure\_message** `string | null` *required*

A merchant-facing explanation of why the refund failed, in the request's
language. Null unless the refund failed or was canceled.

**failure\_reason** `string | null` *required*

Why the refund failed, normalized across providers. Null unless the refund failed or was canceled.

Available options: `bank_declined`, `expired_or_canceled_card`, `lost_or_stolen_card`, `insufficient_funds`, `charge_disputed`, `not_refundable`, `merchant_request`, `unknown`

**metadata** `object | null` *required*

Your own key-value data attached when the refund was requested.

**original\_amount** `object` *required*

The refunded amount in the currency the processor moved.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**payment\_id** `string` *required*

The payment this refund reverses, prefixed `pay_`.

**provider** `string` *required*

The payment provider that processed the refund, such as `paypal` or
`coinbase`.

**provider\_created\_at** `string | null` *required*

When the provider created the refund, as an ISO 8601 timestamp.

**reason** `string | null` *required*

Why the refund was issued, when recorded.

Available options: `duplicate`, `fraudulent`, `requested_by_customer`, `expired_uncaptured_charge`, `dispute_alert`

**reference\_status** `string | null` *required*

Whether a banking-network tracking reference is available for this refund.

Available options: `available`, `pending`, `unavailable`

**reference\_type** `string | null` *required*

The kind of tracking reference, such as an acquirer reference number.

Available options: `acquirer_reference_number`, `retrieval_reference_number`, `system_trace_audit_number`

**reference\_value** `string | null` *required*

The tracking reference the buyer's bank can trace the refund by.

**status** `string` *required*

Where the refund stands with the processor: `pending`, `requires_action`, `succeeded`, `failed`, or `canceled`.

Available options: `pending`, `requires_action`, `succeeded`, `failed`, `canceled`

**updated\_at** `string` *required*

When the refund last changed, as an ISO 8601 timestamp.

**visa\_rdr** `boolean` *required*

True when the card network initiated the refund through Rapid Dispute
Resolution.

```json Refund theme={null}
{
	"id": "rf_xxxxxxxxxxxxxx",
	"payment_id": "pay_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"status": "succeeded",
	"amount": {
		"amount": "49.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"original_amount": {
		"amount": "49.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"provider": "stripe",
	"reason": "requested_by_customer",
	"failure_reason": null,
	"failure_message": null,
	"reference_type": "acquirer_reference_number",
	"reference_status": "available",
	"reference_value": "74000006259000123456789",
	"visa_rdr": false,
	"metadata": {
		"refund_id": "ref_8f2k1"
	},
	"provider_created_at": "2026-09-15T14:30:02.000Z",
	"created_at": "2026-09-15T14:30:00.000Z",
	"updated_at": "2026-09-15T14:30:05.000Z"
}
```


## Related topics

- [Refund](https://docs.whop.com/api-reference/legacy/refunds/refund.md)
- [List refunds](https://docs.whop.com/api-reference/legacy/refunds/list-refunds.md)
- [Retrieve refund](https://docs.whop.com/api-reference/legacy/refunds/retrieve-refund.md)
