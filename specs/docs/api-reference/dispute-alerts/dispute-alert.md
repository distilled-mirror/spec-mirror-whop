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

# Dispute Alerts API

> A Dispute alert is an early warning from a card issuer that a settled payment is being questioned, ahead of any chargeback.

`type` separates fraud reports (`early_fraud_warning`), pre-dispute notices (`dispute_alert`), and Visa RDR cases the network already closed by refunding (`rapid_dispute_resolution`).

Use the Dispute alerts API to list alerts for an account, filter them by type or payment, and read `actionable` to see whether refunding can still avoid the chargeback.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Dispute Alerts](https://docs.whop.com/api-reference/dispute-alerts/list-dispute-alerts.md) | GET `/dispute_alerts` |
| [Retrieve Dispute Alert](https://docs.whop.com/api-reference/dispute-alerts/retrieve-dispute-alert.md) | GET `/dispute_alerts/{id}` |

## Attributes

**id** `string` *required*

Dispute alert ID, prefixed `dspa_`.

**account\_id** `string | null` *required*

The account the alerted payment belongs to, prefixed `biz_`. `null` while the
alert is unmatched.

**amount** `number` *required*

The alerted amount, in whole units of `currency`. This is what the issuer
reported, which can differ from the payment's own amount.

**auto\_refunded** `boolean` *required*

Whether Whop automatically refunded the alerted payment. Reflects the payment,
so it can be `true` for a refund issued by another flow (RDR, resolution) on
the same payment.

**card\_brand** `string | null` *required*

The card network as reported by the issuer, lowercased, such as `visa` or
`mastercard`. `unknown` when the report carries neither a network nor a
recognizable BIN.

**created\_at** `string` *required*

When Whop received the alert, as an ISO 8601 timestamp.

**currency** `string` *required*

Three-letter ISO currency code of the alerted amount.

**fee** `object | null` *required*

The alert fee charged to the account, in the currency it was collected in. `null` when `fee_charged` is false.

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

**fee\_charged** `boolean` *required*

Whether Whop charged the account an alert fee for this one. Always `false` for
`early_fraud_warning`, which Whop is not billed for and never passes on.

**issuer** `string | null` *required*

Deprecated: always `null` outside Whop's own dashboard. Name of the bank that
issued the card and filed the report. DEPRECATED: Always null outside Whop's
own dashboard.

**payment\_id** `string | null` *required*

The payment the issuer reported, prefixed `pay_`. `null` when Whop could not
match the report to a payment.

**product\_id** `string | null` *required*

The product the alerted payment was for, prefixed `prod_`.

**reason** `string | null` *required*

What the cardholder claims. `fraud` when they say they didn't make the payment: always for an `early_fraud_warning`, and for Ethoca fraud alerts and Visa RDR cases filed under a fraud reason. `customer_dispute` when they made the payment but dispute it, such as a product not received or a canceled subscription. `null` when the alert carries no claim, such as an issuer's request for transaction documents.

Available options: `fraud`, `customer_dispute`

**reported\_at** `string` *required*

When the issuer filed the report, as an ISO 8601 timestamp. Earlier than
`created_at`, which is when Whop received it.

**source** `string | null` *required*

The network program that sent the alert, which `type` alone doesn't tell apart. `ethoca` for Mastercard Ethoca, `cdrn` for Visa Verifi CDRN, `rdr` for Visa RDR, `tc40` / `safe` for a Visa / Mastercard fraud report, `paypal` for a PayPal alert. `null` when the network didn't say.

Available options: `ethoca`, `cdrn`, `rdr`, `tc40`, `safe`, `paypal`

**transaction\_at** `string | null` *required*

When the reported transaction was made, as an ISO 8601 timestamp — falls back
to when the matched payment was made if the issuer's own report didn't carry
one. Should not be `null` in practice; treat one as a data issue rather than
expected behavior.

**type** `string` *required*

What the issuer sent. `early_fraud_warning` is a fraud report on a settled payment (Visa TC40 / Mastercard SAFE) — refunding still avoids the chargeback, and Whop never charges a fee for one. `dispute_alert` is a pre-dispute notice from the issuer's alert network, which Whop pays for and passes on as a fee. `rapid_dispute_resolution` is a Visa RDR case the network already closed by refunding the payment — nothing is left to act on.

Available options: `early_fraud_warning`, `dispute_alert`, `rapid_dispute_resolution`

**updated\_at** `string` *required*

When the alert was last changed, as an ISO 8601 timestamp.

```json DisputeAlert theme={null}
{
	"id": "dspa_xxxxxxxxxxxxx",
	"type": "dispute_alert",
	"source": "ethoca",
	"reason": "fraud",
	"amount": 69.95,
	"currency": "usd",
	"card_brand": "visa",
	"issuer": null,
	"account_id": "biz_xxxxxxxxxxxxxx",
	"payment_id": "pay_xxxxxxxxxxxxxx",
	"product_id": "prod_xxxxxxxxxxxxx",
	"fee_charged": true,
	"fee": {
		"amount": "20.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"auto_refunded": false,
	"reported_at": "2026-08-10T17:37:09.000Z",
	"transaction_at": "2026-08-02T00:00:00.000Z",
	"created_at": "2026-08-10T21:39:26.668Z",
	"updated_at": "2026-08-10T21:39:28.137Z"
}
```


## Related topics

- [Dispute Alert](https://docs.whop.com/api-reference/legacy/dispute-alerts/dispute-alert.md)
- [List dispute alerts](https://docs.whop.com/api-reference/legacy/dispute-alerts/list-dispute-alerts.md)
- [Retrieve dispute alert](https://docs.whop.com/api-reference/legacy/dispute-alerts/retrieve-dispute-alert.md)
