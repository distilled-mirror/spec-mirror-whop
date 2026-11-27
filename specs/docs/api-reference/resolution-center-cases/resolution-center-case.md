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

# Resolution Center Cases API

> A Resolution Center Case is opened by a buyer when something is wrong with a purchase, such as an unwanted renewal, an item that never arrived, or a charge they don't recognize.

It is the step before a chargeback: the buyer and merchant work it out directly, and Whop decides the case if they can't.

Use the Resolution Center Cases API from either side: as the buyer, open, reply to, appeal, or withdraw a case; as the merchant, accept it (refunding the payment), deny it, or ask the buyer for more information.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Resolution Center Cases](https://docs.whop.com/api-reference/resolution-center-cases/list-resolution-center-cases.md) | GET `/resolution_center_cases` |
| [Retrieve Resolution Center Case](https://docs.whop.com/api-reference/resolution-center-cases/retrieve-resolution-center-case.md) | GET `/resolution_center_cases/{id}` |
| [List Resolution Center Case Events](https://docs.whop.com/api-reference/resolution-center-cases/list-resolution-center-case-events.md) | GET `/resolution_center_cases/{id}/events` |
| [Retrieve Resolution Center Case Summary](https://docs.whop.com/api-reference/resolution-center-cases/retrieve-resolution-center-case-summary.md) | GET `/resolution_center_cases/summary` |
| [Open a Case as the Customer](https://docs.whop.com/api-reference/resolution-center-cases/open-a-case-as-the-customer.md) | POST `/resolution_center_cases` |
| [Accept a Case](https://docs.whop.com/api-reference/resolution-center-cases/accept-a-case.md) | POST `/resolution_center_cases/{id}/accept` |
| [Appeal a Case](https://docs.whop.com/api-reference/resolution-center-cases/appeal-a-case.md) | POST `/resolution_center_cases/{id}/appeal` |
| [Deny a Case](https://docs.whop.com/api-reference/resolution-center-cases/deny-a-case.md) | POST `/resolution_center_cases/{id}/deny` |
| [Reply to a Case](https://docs.whop.com/api-reference/resolution-center-cases/reply-to-a-case.md) | POST `/resolution_center_cases/{id}/reply` |
| [Request Information from the Customer](https://docs.whop.com/api-reference/resolution-center-cases/request-information-from-the-customer.md) | POST `/resolution_center_cases/{id}/request_info` |
| [Withdraw a Case](https://docs.whop.com/api-reference/resolution-center-cases/withdraw-a-case.md) | POST `/resolution_center_cases/{id}/withdraw` |

## Attributes

**id** `string` *required*

Resolution center case ID, prefixed `reso_`.

**account** `object | null` *required*

The account the case was filed against.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **title** `string` *required*
>
> Account display name.

**amount** `number` *required*

The amount in question, in whole units of `currency`.

**answers** `object[]` *required*

The customer's answers to the questions asked when they opened the case, in the order asked. Empty for a case opened without them.

> [accordion] Properties
>
> **id** `string` *required*
>
> Unique identifier for the answer, prefixed `inrqi_`.
>
> **attachments** `object[]` *required*
>
> Files the customer added to this answer, such as screenshots.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Unique identifier for the attachment, prefixed `att_`.
> >
> > **content\_type** `string | null` *required*
> >
> > The file's MIME type.
> >
> > **filename** `string | null` *required*
> >
> > The original file name.
> >
> > **url** `string | null` *required*
> >
> > A URL to view or download the file.
>
> **question** `string` *required*
>
> The question this answers. Which questions a case asks depends on its `reason` and on earlier answers.
>
> Available options: `purchased`, `recognizes_purchase`, `complaint_kind`, `missing_items`, `expected_by`, `promised`, `promised_link`, `received_instead`, `quality_issue`, `contacted_merchant`, `contacted_on`, `merchant_reply`, `extra`
>
> **value** `string | null` *required*
>
> The customer's answer. Choice questions hold one of their fixed values, and `contacted_on` holds an ISO 8601 date. `null` when an optional question was skipped.

**available\_actions** `string[]` *required*

What you can do to this case right now, named for the endpoint that does it. Resolved for the calling credential, so a merchant and a customer reading the same case see their own options.

Available options: `accept`, `deny`, `request_info`, `reply`, `appeal`, `withdraw`

**buyer** `object` *required*

The customer who opened the case.

> [accordion] Properties
>
> **email** `string | null` *required*
>
> The customer's email address. Requires the `member:email:read` scope; `null`
> without it.
>
> **member\_id** `string | null` *required*
>
> The customer's member row on the account, prefixed `mem_`.
>
> **name** `string | null` *required*
>
> The customer's display name.
>
> **user\_id** `string | null` *required*
>
> The customer's user ID, prefixed `user_`.
>
> **username** `string | null` *required*
>
> The customer's Whop username.

**closed\_at** `string | null` *required*

When the case closed, as an ISO 8601 timestamp. `null` while the case is open,
and on older closed cases that predate this being recorded.

**created\_at** `string` *required*

When the case was opened, as an ISO 8601 timestamp.

**currency** `string | null` *required*

Three-letter ISO currency code of the amount.

**customer\_appealed** `boolean` *required*

Whether the customer has appealed a decision on this case.

**escalated** `boolean` *required*

Whether Whop is involved — either reviewing the case, or waiting on the side
named by `status` for something it asked for while reviewing.

**line\_items** `object[]` *required*

Everything the disputed payment charged for, in purchase order. `product_id` and `variant_id` name the first of these; a cart's later items appear only here. A payment made before items were recorded lists the single item its variant implies. Empty when the payment is not linked to a variant.

> [accordion] Properties
>
> **id** `string | null` *required*
>
> Line item ID, prefixed `li_`. Null when the payment predates item snapshots
> and the item is read from the payment's variant.
>
> **discount\_amount** `object | null` *required*
>
> The amount taken off this item's `subtotal` by discounts and the promo code, before tax. Returns `null` when no discount was recorded for the item.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **label** `string | null` *required*
>
> The item's name as shown at checkout (the product title, else the variant
> title).
>
> **product\_id** `string | null` *required*
>
> The product the variant belongs to, prefixed `prod_`. On a payment that
> predates item snapshots this falls back to the variant's product, so it can be
> set where the parent's own `product_id` is null. Null for a variant with no
> product.
>
> **product\_title** `string | null` *required*
>
> The product's current title, or `null` when the item has no product.
>
> **quantity** `number` *required*
>
> How many units were bought.
>
> **sku** `string | null` *required*
>
> The variant's stock keeping unit, free text set by the seller (e.g.
> `TSHIRT-LARGE-BLUE`). Accounts with billing-reason SKU suffixes enabled
> receive it stamped with why the charge happened: `-S` for a first subscription
> charge, `-R` for a renewal, `-C` for a one-time purchase, `-U` when the
> payment's own metadata carries `upsell: true`, `-F` when nothing was charged.
> Other billing reasons carry the bare SKU. `null` when the variant has no SKU
> or has been deleted.
>
> **subtotal** `object | null` *required*
>
> The recorded amount for this item's full quantity, before discounts, tax, and fees, in its purchase currency. Returns `null` when no item amount was recorded.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **variant\_id** `string | null` *required*
>
> The variant bought, prefixed `plan_`. Null when the variant has since been
> deleted.
>
> **variant\_title** `string | null` *required*
>
> The variant's current title, or `null` when the variant has been deleted or has no title.

**outcome** `string | null` *required*

Who prevailed on the claim. `null` until the case closes. Read `refund` for whether any money actually moved.

Available options: `customer_won`, `merchant_won`, `withdrawn`

**payment** `object` *required*

The payment the case was opened against.

> [accordion] Properties
>
> **id** `string` *required*
>
> Payment ID, prefixed `pay_`.
>
> **card\_brand** `string | null` *required*
>
> Card brand, when the customer paid by card.
>
> **card\_last4** `string | null` *required*
>
> Last four digits of the card, when the customer paid by card.
>
> **created\_at** `string` *required*
>
> When the payment was made, as an ISO 8601 timestamp.
>
> **payment\_method\_type** `string | null` *required*
>
> How the customer paid, such as `card` or `paypal`.

**product\_id** `string | null` *required*

The product the payment was for, prefixed `prod_`.

**reason** `string` *required*

What the customer says went wrong. Shares the `/disputes` vocabulary, so a case that later becomes a chargeback reports the same complaint.

Available options: `fraudulent`, `product_not_received`, `not_as_described`, `product_unacceptable`, `subscription_canceled`, `credit_not_processed`

**refund** `string | null` *required*

Whether money moved and off whose balance: `none`, `merchant`, or `platform` (Whop refunded the customer and the merchant kept the funds). Independent of `outcome` — a case the merchant won can still carry a platform refund. `null` while the case is open, and on older closed cases that predate this being recorded.

Available options: `none`, `merchant`, `platform`

**response\_due\_at** `string | null` *required*

When the next response is due, as an ISO 8601 timestamp.

**status** `string` *required*

Who the case is waiting on. `awaiting_merchant` and `awaiting_customer` name the side that owes a response, `under_review` means Whop is deciding, and `closed` means it is settled — read `outcome` for how.

Available options: `awaiting_merchant`, `awaiting_customer`, `under_review`, `closed`

**updated\_at** `string` *required*

When the case was last changed, as an ISO 8601 timestamp.

**variant\_id** `string | null` *required*

The variant the payment was made on, prefixed `plan_`.

```json ResolutionCenterCase theme={null}
{
	"id": "reso_xxxxxxxxxxxxxx",
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"amount": 249,
	"answers": [
		{
			"id": "inrqi_xxxxxxxxxxxxxx",
			"question": "complaint_kind",
			"value": "never_received",
			"attachments": [
				{
					"id": "att_xxxxxxxxxxxxxx",
					"filename": "booking-confirmation.png",
					"content_type": "image/png",
					"url": "https://example.com/booking-confirmation.png"
				}
			]
		}
	],
	"available_actions": ["accept", "deny", "request_info", "reply"],
	"buyer": {
		"email": "dana@shinetime.example",
		"member_id": "mber_xxxxxxxxxxxxxx",
		"name": "Dana Whitfield",
		"user_id": "user_xxxxxxxxxxxxxx",
		"username": "danawhitfield"
	},
	"closed_at": null,
	"created_at": "2026-09-15T14:30:00.000Z",
	"currency": "usd",
	"customer_appealed": false,
	"escalated": false,
	"line_items": [
		{
			"id": "li_xxxxxxxxxxxxxx",
			"label": "Ceramic Coating Package",
			"variant_id": "plan_xxxxxxxxxxxxxx",
			"variant_title": "Ceramic Coating — Full Vehicle",
			"product_id": "prod_xxxxxxxxxxxxxx",
			"product_title": "Ceramic Coating Package",
			"quantity": 1,
			"sku": null,
			"subtotal": {
				"amount": "249.00",
				"currency": "usd",
				"decimals": 2,
				"display_decimals": 2
			},
			"discount_amount": null
		}
	],
	"outcome": null,
	"payment": {
		"id": "pay_xxxxxxxxxxxxxx",
		"card_brand": "visa",
		"card_last4": "4242",
		"created_at": "2026-09-02T10:00:00.000Z",
		"payment_method_type": "card"
	},
	"variant_id": "plan_xxxxxxxxxxxxxx",
	"product_id": "prod_xxxxxxxxxxxxxx",
	"reason": "product_not_received",
	"refund": null,
	"response_due_at": "2026-09-18T14:30:00.000Z",
	"status": "awaiting_merchant",
	"updated_at": "2026-09-15T14:30:00.000Z"
}
```


## Related topics

- [Resolution Center Case](https://docs.whop.com/api-reference/legacy/resolution-center-cases/resolution-center-case.md)
- [List resolution center cases](https://docs.whop.com/api-reference/legacy/resolution-center-cases/list-resolution-center-cases.md)
- [Retrieve resolution center case](https://docs.whop.com/api-reference/legacy/resolution-center-cases/retrieve-resolution-center-case.md)
