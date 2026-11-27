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

# Payouts API

> Payouts represent money sent from an account or user balance to an external destination, such as a bank account, wallet, or other saved payout method.

Use the Payouts API to create and track payouts, manage saved payout methods, and show expected arrival details for funds leaving Whop.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Payouts](https://docs.whop.com/api-reference/payouts/list-payouts.md) | GET `/payouts` |
| [Retrieve Payout](https://docs.whop.com/api-reference/payouts/retrieve-payout.md) | GET `/payouts/{id}` |
| [List Saved Payout Methods](https://docs.whop.com/api-reference/payouts/list-saved-payout-methods.md) | GET `/payouts/methods` |
| [List Supported Payout Methods](https://docs.whop.com/api-reference/payouts/list-supported-payout-methods.md) | GET `/payouts/supported_methods` |
| [Create Payout](https://docs.whop.com/api-reference/payouts/create-payout.md) | POST `/payouts` |
| [Cancel Payout](https://docs.whop.com/api-reference/payouts/cancel-payout.md) | POST `/payouts/{id}/cancel` |
| [Create Saved Payout Method](https://docs.whop.com/api-reference/payouts/create-saved-payout-method.md) | POST `/payouts/methods` |
| [Create PayPal Link Session](https://docs.whop.com/api-reference/payouts/create-paypal-link-session.md) | POST `/payouts/paypal_link_sessions` |
| [Create Payout Quote](https://docs.whop.com/api-reference/payouts/create-payout-quote.md) | POST `/payouts/quotes` |
| [Update Saved Payout Method](https://docs.whop.com/api-reference/payouts/update-saved-payout-method.md) | PATCH `/payouts/methods/{id}` |
| [Delete Saved Payout Method](https://docs.whop.com/api-reference/payouts/delete-saved-payout-method.md) | DELETE `/payouts/methods/{id}` |

## Attributes

**id** `string` *required*

Payout ID, prefixed `wdrl_`.

**amount** `string` *required*

The payout amount in whole currency units, as a decimal string.

**cancelable** `boolean` *required*

Whether `POST /payouts/\{id}/cancel` would act on this payout right now:
`true` while it is `in_review`, while it is `processing` with nothing yet
submitted to the payout provider, or while a submitted payout can still be
asked to cancel at the provider. `false` once the money has arrived or the
payout has already ended.

**conversion** `object | null` *required*

How the balance was converted into the payout currency before the payout was sent. `null` when the payout was sent from a balance in its own currency.

> [accordion] Properties
>
> **from\_amount** `string` *required*
>
> The amount the conversion took from the balance, as a decimal string.
>
> **from\_currency** `string` *required*
>
> The currency of the balance the conversion took the amount from.
>
> **rate** `number` *required*
>
> The conversion rate, in units of the payout currency for each unit of `from_currency`. Any conversion charge is included in this rate.

**created\_at** `string` *required*

When the payout was created.

**currency** `string` *required*

Payout currency.

**destination\_amount** `string | null` *required*

The amount delivered in the destination currency, as a decimal string.
Assigned when the payout is processed, so it is `null` before then and on
payouts without a recorded conversion.

**destination\_currency** `string | null` *required*

Currency the funds are delivered in, taken from the payout method when the
payout is created. On a payout funded by a conversion request it follows the
settlement payout minted alongside it — the `GET /payouts` row carrying this
payout's id as `payout_request_id` — and is `null` only when no settlement
payout exists.

**estimated\_arrival** `string | null` *required*

Estimated time the funds become available in the destination account.

**exchange\_rate** `number | null` *required*

Exchange rate from the payout currency to the destination currency. Assigned
when the payout is processed, so it is `null` before then and on payouts
without a recorded rate.

**failure** `object | null` *required*

Why the payout ended without paying, or why it reversed after settlement. Present on failed, canceled, denied, and reversed payouts; `null` otherwise.

> [accordion] Properties
>
> **code** `string | null` *required*
>
> Classified failure code from the maintained error catalog.
>
> **funds\_returned\_at** `string | null` *required*
>
> The effective time of the reversal that put the funds back in the balance —
> `null` if they never left it or have not returned yet. Set only once the
> return is confirmed in the ledger; the ledger posting itself can land moments
> after this time.
>
> **message** `string | null` *required*
>
> Human-readable explanation of the failure. Callers holding
> `payout:destination:read` may receive text personalized to the destination.
> Other callers get the generic catalog message.
>
> **next\_step** `string | null` *required*
>
> What to do before a new payout can succeed. `withdraw_again` means that the payout method needs no change. `null` when the failure has no code from the error catalog, or when the account owner canceled the payout.
>
> Available options: `update_payout_method`, `use_another_payout_method`, `complete_verification`, `withdraw_again`, `contact_bank`, `contact_support`

**fee\_amount** `string` *required*

The fee charged for the payout, in the payout currency, as a decimal string.

**fee\_paid\_by** `string` *required*

Who bore the payout fee: the account itself, or its parent platform.

Available options: `self`, `platform`

**markup\_fee** `string` *required*

Whop's markup on the provider fee, in the payout currency, as a decimal
string. `"0.0"` when none applies.

**metadata** `object` *required*

Key-value data attached at creation and echoed on every read. At most 50 keys,
key names up to 40 characters, string values up to 500 characters.

**net\_amount** `string` *required*

The planned net for the destination, in the payout currency: amount minus
fee\_amount minus markup\_fee when fee\_paid\_by is `self`; equal to amount when
the platform covers the fees. A payout that ends denied, canceled, or failed
delivered nothing — most keep the planned figure and `failure` says where the
funds are, but a canceled payout funded by a conversion request can report the
settled outcome instead: `amount` carries what stayed in the balance, fees are
zero because none were charged, and `net_amount` is 0 because nothing was
delivered.

**notes** `string | null` *required*

Free-form notes attached by the payout creator, or `null` when none were
provided. Maximum 255 characters.

**object** `string` *required*

**payer\_name** `string | null` *required*

Name of the entity processing the payout.

**payout\_method** `object | null` *required*

The saved payout method used. Requires payout:destination:read; null without it.

> [accordion] Properties
>
> **nickname** `string | null` *required*
>
> Saved payout method nickname.
>
> **supported\_payout\_method** `object | null` *required*
>
> Supported payout method display details.
>
> > [accordion] Properties
> >
> > **delivery\_type** `string` *required*
> >
> > How the funds are delivered to the recipient.
> >
> > Available options: `cash_pickup`, `bank_deposit`, `home_delivery`, `mobile_wallet`, `card`, `check`, `bill`, `cryptocurrency`, `unknown`
> >
> > **icon\_url** `string | null` *required*
> >
> > Supported payout method icon URL.
> >
> > **payer\_name** `string | null` *required*
> >
> > Supported payout method display name.
>
> **type** `string` *required*
>
> The kind of payout method the payout used. `external` sent funds out of Whop. `whop_card` moved funds onto the account's own Whop Card.
>
> Available options: `external`, `whop_card`

**payout\_request\_id** `string | null` *required*

Payout request ID, prefixed `cofr_`, returned by `POST /payouts`. Match it to
the settled payout in `GET /payouts`. Returns `null` for payouts not created
by `POST /payouts`.

**source** `string | null` *required*

How the payout was created. `automatic` means a scheduled auto-payout. `null` when the payout has no recorded source.

Available options: `api`, `dashboard`, `automatic`

**speed** `string` *required*

Payout delivery speed.

Available options: `standard`, `instant`

**statement\_descriptor** `string | null` *required*

The payment description sent with the payout, or `null` if no descriptor was
set. When set, 5-80 printable ASCII characters.

**status** `string` *required*

Current payout status.

Available options: `requested`, `in_review`, `processing`, `completed`, `reversed`, `canceled`, `failed`, `denied`

**status\_detail** `string` *required*

The finest machine phase under `status` — for example
`awaiting_provider_acceptance` vs `in_transit` under `processing`, or the
conversion phase under `requested`. Informational vocabulary: values can be
added without a version bump; `status` is the versioned contract.

**trace\_code** `string | null` *required*

Payment network reference for this payout, such as an ACH trace number, an RTP
or FedNow end-to-end ID, a wire IMAD, or a SWIFT UETR. `null` until the
reference is available, and on payouts that have none.

```json Payout theme={null}
{
	"id": "wdrl_xxxxxxxxxxxxx",
	"amount": "125.5",
	"cancelable": false,
	"conversion": null,
	"created_at": "2026-06-01T12:00:00Z",
	"currency": "usd",
	"destination_amount": "115.89",
	"destination_currency": "eur",
	"estimated_arrival": "2026-06-03T12:00:00Z",
	"exchange_rate": 0.9234,
	"failure": null,
	"fee_amount": "1.25",
	"fee_paid_by": "self",
	"markup_fee": "0.0",
	"metadata": {
		"batch_id": "2026-06-01"
	},
	"net_amount": "124.25",
	"notes": "March affiliate commissions",
	"object": "payout",
	"payer_name": "Whop Payments",
	"payout_request_id": "cofr_xxxxxxxxxxxxx",
	"payout_method": {
		"nickname": "Operating account",
		"type": "external",
		"supported_payout_method": {
			"delivery_type": "bank_deposit",
			"icon_url": "https://cdn.whop.com/bank.png",
			"payer_name": "Whop Payments"
		}
	},
	"source": "api",
	"speed": "standard",
	"status": "completed",
	"status_detail": "completed",
	"statement_descriptor": "MYCOMPANY",
	"trace_code": "021000021234567"
}
```


## Related topics

- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Manual Payouts to Subaccounts](https://docs.whop.com/developer/platforms/manual-payouts.md)
- [Payout created](https://docs.whop.com/api-reference/payouts/payout-created.md)
