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

# Invoices API

> An Invoice is a receipt with an invoice number. Create a draft from variant-backed items, set its recipient and due date, and edit it until it is sent.

Its `pay_` ID is also the payment ID when collected.

Use the Invoices API to [create](https://docs.whop.com/api-reference/invoices/create-invoice.md) and edit drafts, then [issue](https://docs.whop.com/api-reference/invoices/issue-invoice.md) or [send](https://docs.whop.com/api-reference/invoices/send-invoice.md) them. You can [charge](https://docs.whop.com/api-reference/invoices/charge-invoice.md) an open invoice to a saved payment method, or [void](https://docs.whop.com/api-reference/invoices/void-invoice.md) an invoice the buyer will not pay.

Draft creation does not create access or charge the buyer. The recipient's contact details live on the account's member. Financial fields and caller metadata can only be edited in draft.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Invoices](https://docs.whop.com/api-reference/invoices/list-invoices.md) | GET `/invoices` |
| [Retrieve Invoice](https://docs.whop.com/api-reference/invoices/retrieve-invoice.md) | GET `/invoices/{id}` |
| [List Invoice Sends](https://docs.whop.com/api-reference/invoices/list-invoice-sends.md) | GET `/invoices/{id}/sends` |
| [Create Invoice](https://docs.whop.com/api-reference/invoices/create-invoice.md) | POST `/invoices` |
| [Charge Invoice](https://docs.whop.com/api-reference/invoices/charge-invoice.md) | POST `/invoices/{id}/charge` |
| [Generate Invoice PDF](https://docs.whop.com/api-reference/invoices/generate-invoice-pdf.md) | POST `/invoices/{id}/generate_pdf` |
| [Issue Invoice](https://docs.whop.com/api-reference/invoices/issue-invoice.md) | POST `/invoices/{id}/issue` |
| [Mark Invoice Uncollectible](https://docs.whop.com/api-reference/invoices/mark-invoice-uncollectible.md) | POST `/invoices/{id}/mark_uncollectible` |
| [Send Invoice](https://docs.whop.com/api-reference/invoices/send-invoice.md) | POST `/invoices/{id}/send` |
| [Cancel Scheduled Send](https://docs.whop.com/api-reference/invoices/cancel-scheduled-send.md) | POST `/invoices/{id}/unschedule` |
| [Void Invoice](https://docs.whop.com/api-reference/invoices/void-invoice.md) | POST `/invoices/{id}/void` |
| [Update Invoice](https://docs.whop.com/api-reference/invoices/update-invoice.md) | PATCH `/invoices/{id}` |
| [Delete Invoice](https://docs.whop.com/api-reference/invoices/delete-invoice.md) | DELETE `/invoices/{id}` |

## Attributes

**id** `string` *required*

**account** `object` *required*

The account that sent the invoice.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **title** `string` *required*
>
> Account display name.

**account\_id** `string` *required*

**amount\_due** `object` *required*

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

**billing\_address** `object | null` *required*

> [accordion] Properties
>
> **city** `string | null` *required*
>
> The city.
>
> **country** `string | null` *required*
>
> The ISO 3166-1 alpha-2 country code.
>
> **line1** `string | null` *required*
>
> The first street address line.
>
> **line2** `string | null` *required*
>
> The second street address line.
>
> **name** `string | null` *required*
>
> The name on the address.
>
> **postal\_code** `string | null` *required*
>
> The postal or ZIP code.
>
> **state** `string | null` *required*
>
> The state, province or region.

**buyer\_fee\_amount** `object` *required*

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

**buyer\_fee\_percentage** `number` *required*

**charge\_at\_subscription\_start** `boolean` *required*

True when the customer pays only the initial fee of each item now and is first
charged the renewal price at `subscription_starts_at`. Until then the
membership is `scheduled` and has no access.

**checkout\_link** `string | null` *required*

A signed link that opens this invoice in a checkout session so the buyer can
pay it. Pass it as `link` to `POST /checkout_sessions`. Present while the
invoice is open. `null` in any other status, for an invoice billed to a
business buyer, and for an invoice whose membership was canceled. Also `null`
unless you have `payment:charge`, or `invoice:create` or `invoice:update` on
an invoice with no `payment_method_id`.

**created\_at** `string` *required*

**customer\_name** `string | null` *required*

**deletable** `boolean` *required*

True when `DELETE /invoices/\{id}` can delete this invoice. A draft qualifies
unless it is scheduled or bills a subscription. A void invoice qualifies
unless it collected money or still awaits a bank wire.

**due\_at** `string | null` *required*

**email\_address** `string | null` *required*

**invoice\_prefix** `string | null` *required*

**issued\_at** `string | null` *required*

When the invoice was issued, as an ISO 8601 timestamp. This is when it left
draft, which can be after `created_at` for an invoice drafted or scheduled
ahead. `null` while the invoice is a draft, and for invoices issued before
this field was introduced.

**line\_items** `object[]` *required*

> [accordion] Properties
>
> **id** `string | null` *required*
>
> Line item ID, prefixed `li_`. Null when the payment predates item snapshots
> and the item is read from the payment's variant.
>
> **description** `string | null` *required*
>
> Text shown under the item on the invoice, its email, and its PDF, read from
> the variant's `description`. A custom charge sets it with the item's
> `description`. A product variant keeps its own. Null when the variant has none
> or has since been deleted.
>
> **discount\_amount** `object | null` *required*
>
> The promo discount applied to this item, in the payment's `currency`. Null when no discount was recorded.
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
> **initial\_price** `object | null` *required*
>
> The part of `unit_amount` that is a recurring variant's one-time upfront fee, the variant's `initial_price`, charged on this invoice only. The rest of `unit_amount` is the first billing period. A trial has no first-period charge. Null for one-time items, renewal invoices, items with no upfront fee, and items whose variant prices changed after the invoice was priced.
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
> The product the variant belongs to, prefixed `prod_`. Null for a variant with
> no product.
>
> **quantity** `number` *required*
>
> How many units were bought.
>
> **subtotal** `object | null` *required*
>
> `unit_amount` times `quantity`, before discounts and tax, in the payment's `currency`.
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
> **tax\_amount** `object | null` *required*
>
> The tax collected on this item, in the payment's `currency`. Null when no tax was recorded.
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
> **trial\_period\_days** `integer | null` *required*
>
> Free trial days before this recurring item's first renewal charge, from the
> variant's `trial_period_days`. The invoice then bills only the variant's
> `initial_price` for the item. `null` for one-time items and renewal invoices,
> and when the customer already had a subscription to the variant.
>
> **unit\_amount** `object | null` *required*
>
> The saved price of one unit before discounts, in the payment's `currency`. Creating or replacing recurring items includes the upfront fee, plus the first billing period unless the item starts with a trial. Renewal invoices exclude the upfront fee. Other edits preserve saved prices. Null when the payment recorded no price.
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
> The variant bought, prefixed `plan_`. Null when the variant has since been deleted.

**mailing\_address\_id** `string | null` *required*

**member\_id** `string | null` *required*

**membership\_id** `string | null` *required*

The membership this invoice bills, prefixed `mem_`. For a renewal invoice, it
is the subscription that renews. For an invoice with a recurring item, it is
the membership created when the invoice is issued. Null for one-off invoices,
for a draft not yet issued, and without the member:basic:read permission.

**memo** `string | null` *required*

A note to the customer, shown on the invoice email, PDF, and checkout page
with its line breaks kept. `null` when none is set.

**metadata** `object` *required*

Key-value pairs the seller stored on the invoice. Empty when the invoice is
read without a credential.

**number** `string` *required*

**object** `string` *required*

**overdue** `boolean` *required*

**paid\_at** `string | null` *required*

**payment\_method\_configuration** `object | null` *required*

The payment methods the customer can choose from when paying through the invoice's checkout link. It takes precedence over the invoice's variants and the account. `null` when the invoice uses its variants' payment methods.

> [accordion] Properties
>
> **disabled** `string[]` *required*
>
> Payment methods this checkout withholds, even when the defaults would offer
> them.
>
> **enabled** `string[]` *required*
>
> Payment methods this checkout offers on top of whatever the defaults provide.
>
> **include\_platform\_defaults** `boolean` *required*
>
> Whether Whop's default set is the starting point. When `false`, only `enabled` is offered.

**payment\_method\_id** `string | null` *required*

**quote\_accepted\_at** `string | null` *required*

When the customer accepted and signed the quote, as an ISO 8601 timestamp. An
edit to the draft clears it, unless the edit changes only `recipient_emails`.
`null` when the quote was not accepted.

**quote\_amount\_paid** `object | null` *required*

How much of the quote its paid invoices in `quote_invoice_ids` cover, before tax. `null` when the invoice was never sent as a quote.

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

**quote\_amount\_remaining** `object | null` *required*

How much of the quote is not yet billed: its line items before tax, minus every invoice in `quote_invoice_ids`. `null` when the invoice was never sent as a quote.

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

**quote\_deposit\_amount** `object | null` *required*

How much is billed as a deposit when the customer signs, before tax. `null` when the quote has no deposit.

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

**quote\_deposit\_percentage** `number | null` *required*

The share of the quote billed as a deposit when the customer signs, such as
`30` for 30%. `null` when the deposit is a fixed amount or there is none.

**quote\_invoice\_ids** `string[]` *required*

The invoices that bill this quote, such as a deposit and then the balance,
oldest first, each prefixed `pay_`. Empty when the quote was never billed.

**quote\_payment\_url** `string | null` *required*

Where the customer pays the next unpaid invoice in `quote_invoice_ids`. Only
returned when the quote is read with its customer `token`. `null` otherwise,
and when nothing is waiting to be paid.

**quote\_sent\_at** `string | null` *required*

When the draft was last emailed to its recipients as a quote, as an ISO 8601
timestamp. A quote stays a draft and cannot be paid. It is not issued on its
`due_at`. `null` when the invoice was never sent as a quote.

**quote\_signature\_url** `string | null` *required*

A link to the signature the customer drew, as a PNG image with a transparent
background. The link expires after a short time. `null` when the quote was not
accepted, and for a credential without the member:basic:read permission.

**quote\_signer\_email** `string | null` *required*

The email address of the Whop user who signed the quote. `null` when the quote
was not accepted, when the signer was not signed in to Whop, and without the
member:email:read permission.

**quote\_signer\_name** `string | null` *required*

The full name the customer typed to sign the quote. `null` when the quote was
not accepted, and for a credential without the member:basic:read permission.

**recipient\_emails** `string[]` *required*

Invoice email recipients, independent of the billed customer.

**scheduled\_charge\_at** `string | null` *required*

When the invoice's saved payment method will be charged, as an ISO 8601
timestamp. It is the `charge_at` the invoice was sent or issued with. Without
one, it is `due_at` when an invoice with a `payment_method_id` is sent or
issued before it is due. On a draft, it is the `charge_at` of its scheduled
send. `null` otherwise, once the charge starts, and once the invoice is paid,
void, or uncollectible.

**scheduled\_collection\_method** `string | null` *required*

The `collection_method` a draft's scheduled send applies when it fires. `null` when the draft has no scheduled send or it was scheduled without one, and once the invoice is issued.

Available options: `charge_automatically`, `send_invoice`

**scheduled\_send\_at** `string | null` *required*

**sends\_on\_due\_date** `boolean` *required*

Whether this draft is issued automatically when its `due_at` arrives. Issuing
it charges its `payment_method_id`, or emails its payment link to its
`recipient_emails`. `false` for an invoice that is not a draft, for a draft
with a scheduled send, and for a draft sent as a quote. `false` while sending
the draft would fail for missing recipients or an unavailable payment link.
`false` for a draft that had no future `due_at` when it was last saved.
`false` for a draft created before 2026-11-12T22:00:00Z.

**status** `string` *required*

Available options: `draft`, `open`, `authorized`, `paid`, `pending`,
`uncollectible`, `unresolved`, `void`

**subscription\_starts\_at** `string | null` *required*

When the first billing period of the invoice's recurring items starts, as an
ISO 8601 timestamp. Renewals count from this moment. `null` starts the period
when the invoice is paid, and so does a time that has passed by then. An
invoice with a start date skips its variants' free trial. With
`charge_at_subscription_start`, the subscription and its access begin at this
moment instead.

**subtotal** `object` *required*

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

**tax\_amount** `object | null` *required*

The tax charged on the invoice. On a draft, it is an estimate once the draft has a complete billing address, calculated as if it were issued now. Issuing calculates it again, so the final tax can differ. `null` on a draft without a complete billing address, and when tax could not be estimated.

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

**time\_zone** `string` *required*

The IANA time zone for the dates on the invoice's email and PDF, such as
`America/New_York`. `UTC` unless one was set.

**total** `object` *required*

The amount the invoice charges, including tax. On a draft, it includes the estimated `tax_amount`, if any.

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

**totals\_status** `string` *required*

Available options: `estimated`, `final`

**updated\_at** `string` *required*

```json Invoice theme={null}
{
	"id": "pay_xxxxxxxxxxxxxx",
	"object": "invoice",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"member_id": "mber_xxxxxxxxxxxxxx",
	"membership_id": null,
	"number": "#00001042",
	"invoice_prefix": "STAD",
	"status": "open",
	"overdue": false,
	"deletable": false,
	"due_at": "2026-11-30T00:00:00.000Z",
	"subscription_starts_at": null,
	"charge_at_subscription_start": false,
	"scheduled_send_at": null,
	"scheduled_charge_at": null,
	"scheduled_collection_method": "send_invoice",
	"sends_on_due_date": false,
	"quote_sent_at": null,
	"quote_accepted_at": null,
	"quote_signer_name": null,
	"quote_signer_email": null,
	"quote_signature_url": null,
	"quote_invoice_ids": [],
	"quote_amount_remaining": null,
	"quote_amount_paid": null,
	"quote_payment_url": null,
	"quote_deposit_percentage": null,
	"quote_deposit_amount": null,
	"customer_name": "Dana Whitfield",
	"recipient_emails": ["dana@shinetime.example"],
	"email_address": "dana@shinetime.example",
	"payment_method_id": null,
	"mailing_address_id": null,
	"billing_address": {
		"name": "Dana Whitfield",
		"line1": "1114 Bouldin Ave",
		"line2": "Unit B",
		"city": "Austin",
		"state": "TX",
		"postal_code": "78704",
		"country": "US"
	},
	"totals_status": "final",
	"buyer_fee_percentage": 0,
	"subtotal": {
		"currency": "usd",
		"amount": "450.00",
		"decimals": 2,
		"display_decimals": 2
	},
	"buyer_fee_amount": {
		"currency": "usd",
		"amount": "0.00",
		"decimals": 2,
		"display_decimals": 2
	},
	"tax_amount": {
		"currency": "usd",
		"amount": "37.13",
		"decimals": 2,
		"display_decimals": 2
	},
	"total": {
		"currency": "usd",
		"amount": "487.13",
		"decimals": 2,
		"display_decimals": 2
	},
	"amount_due": {
		"currency": "usd",
		"amount": "487.13",
		"decimals": 2,
		"display_decimals": 2
	},
	"checkout_link": "https://whop.com/checkout/pay_xxxxxxxxxxxxxx",
	"line_items": [
		{
			"id": "li_xxxxxxxxxxxxxx",
			"variant_id": "plan_xxxxxxxxxxxxxx",
			"product_id": "prod_xxxxxxxxxxxxxx",
			"label": "Ceramic Coating Package",
			"description": "Full exterior, two-year coating",
			"quantity": 1,
			"trial_period_days": null,
			"unit_amount": {
				"currency": "usd",
				"amount": "450.00",
				"decimals": 2,
				"display_decimals": 2
			},
			"initial_price": null,
			"subtotal": {
				"currency": "usd",
				"amount": "450.00",
				"decimals": 2,
				"display_decimals": 2
			},
			"discount_amount": null,
			"tax_amount": {
				"currency": "usd",
				"amount": "37.13",
				"decimals": 2,
				"display_decimals": 2
			}
		}
	],
	"payment_method_configuration": null,
	"memo": "Thanks for booking with Shine Time.",
	"time_zone": "America/New_York",
	"metadata": {
		"booking_id": "BK-2291"
	},
	"created_at": "2026-11-02T15:00:00.000Z",
	"updated_at": "2026-11-02T15:05:00.000Z",
	"issued_at": "2026-11-02T15:05:00.000Z",
	"paid_at": null
}
```


## Related topics

- [Invoices](https://docs.whop.com/developer/guides/invoices.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Migrate from Legacy Invoices](https://docs.whop.com/developer/guides/migrate-from-legacy-invoices.md)
