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

# Legacy Invoice

> An invoice represents an itemized bill sent by a company to a customer for a specific product and plan, tracking the amount owed, due date, and payment status.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/invoices/list-invoices.md): requests to `/invoices`
> with `Api-Version-Date: 2026-10-24` or later reach native invoices, which the
> [Invoices guide](https://docs.whop.com/developer/guides/invoices.md) covers. This page stays fully
> supported.

```json Example theme={null}
{
	"automatically_finalizes_at": "2023-12-01T05:00:00.401Z",
	"charge_buyer_fee": true,
	"collection_method": "send_invoice",
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"logo": {
			"url": "https://media.whop.com/abc123/optimized.jpg"
		},
		"title": "Shine Time Auto Detailing"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"current_plan": {
		"billing_period": 42,
		"currency": "usd",
		"description": "Monthly maintenance washes, interior vacuuming, and a quarterly sealant top-up.",
		"formatted_price": "$10.00",
		"id": "plan_xxxxxxxxxxxxx",
		"initial_price": 6.9,
		"plan_type": "renewal",
		"renewal_price": 6.9,
		"trial_period_days": 42
	},
	"customer_name": "Dana Whitfield",
	"due_date": "2023-12-01T05:00:00.401Z",
	"email_address": "customer@example.com",
	"fetch_invoice_token": "eyJhbGciOiJIUzI1NiJ9...",
	"id": "inv_xxxxxxxxxxxxxx",
	"line_items": [
		{
			"label": "Platform subscription",
			"position": 42,
			"quantity": 6.9,
			"total": 6.9,
			"unit_price": 6.9
		}
	],
	"mailing_address": {
		"city": "<string>",
		"country": "<string>",
		"line1": "<string>",
		"line2": "<string>",
		"name": "<string>",
		"phone": "<string>",
		"postal_code": "<string>",
		"state": "<string>"
	},
	"member": {
		"id": "<string>"
	},
	"number": "#0001",
	"pay_online_url": "<string>",
	"payment": {
		"id": "pay_xxxxxxxxxxxxxx"
	},
	"payment_processing": true,
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	},
	"status": "draft",
	"subscription_billing_anchor_at": "2023-12-01T05:00:00.401Z",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"user": {
		"email": "john.doe@example.com",
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	}
}
```

**automatically\_finalizes\_at** `string\<date-time\> | null` *required*

The date and time when the invoice will be automatically finalized. For charge\_automatically, triggers an automatic charge. For send\_invoice, sends the invoice email at the specified time.

Example: `2023-12-01T05:00:00.401Z`

**charge\_buyer\_fee** `boolean` *required*

Whether the invoice includes a buyer processing fee on top of the plan price.

**collection\_method** `InvoiceCollectionMethods` *required*

The method used to collect payment for this invoice, such as automatic charging or manual payment.

Available options: `send_invoice`, `charge_automatically`

**company** `object` *required*

The company that issued this invoice.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**logo** `object | null` *required*

The company's logo.

**child attributes**

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**created\_at** `string\<date-time\>` *required*

The datetime the invoice was created.

Example: `2023-12-01T05:00:00.401Z`

**current\_plan** `object` *required*

The plan that this invoice charges for.

**child attributes**

**billing\_period** `integer | null` *required*

Number of days between recurring charges, such as 30 for monthly or 365 for annual. `null` for one-time plans.

Example: `42`

**currency** `Currencies` *required*

The currency used for all prices on this plan (e.g., 'usd', 'eur'). All monetary amounts on the plan are denominated in this currency.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**description** `string | null` *required*

A text description of the plan visible to customers. Maximum 1000 characters. Null if no description is set.

Example: `Monthly maintenance washes, interior vacuuming, and a qua…`

**formatted\_price** `string` *required*

The formatted price (including currency) for the plan.

Example: `$10.00`

**id** `string` *required*

The unique identifier for the plan.

Example: `plan_xxxxxxxxxxxxx`

**initial\_price** `number` *required*

The initial purchase price in the plan's base\_currency (e.g., 49.99 for \$49.99). For one-time plans, this is the full price. For renewal plans, this is charged on top of the first renewal\_price.

Example: `6.9`

**plan\_type** `PlanTypes` *required*

The billing model for this plan: 'renewal' for recurring subscriptions or 'one\_time' for single payments.

Available options: `renewal`, `one_time`

**renewal\_price** `number` *required*

The recurring price charged every billing\_period in the plan's base\_currency (e.g., 9.99 for \$9.99/period). Zero for one-time plans.

Example: `6.9`

**trial\_period\_days** `integer | null` *required*

Free trial days before first renewal charge. `null` if no trial is configured or the user has already used a trial for this plan.

Example: `42`

**customer\_name** `string | null` *required*

The full name of the customer this invoice is addressed to. Null if no name is on file.

Example: `Dana Whitfield`

**due\_date** `string\<date-time\> | null` *required*

The deadline by which payment is expected. Null if the invoice is collected automatically.

Example: `2023-12-01T05:00:00.401Z`

**email\_address** `string | null` *required*

The email address of the customer this invoice is addressed to. Null if no email is on file.

Example: `customer@example.com`

**fetch\_invoice\_token** `string` *required*

A signed token that allows fetching invoice data publicly without authentication.

Example: `eyJhbGciOiJIUzI1NiJ9...`

**id** `string` *required*

The unique identifier for the invoice.

Example: `inv_xxxxxxxxxxxxxx`

**line\_items** `array\<object\>` *required*

Optional line items that break down the invoice total into individual charges.

**child attributes**

**label** `string` *required*

The label or description for this line item.

Example: `Platform subscription`

**position** `integer` *required*

The display order of this line item within the invoice.

Example: `42`

**quantity** `number` *required*

The quantity of this line item.

Example: `6.9`

**total** `number` *required*

The computed total for this line item (quantity \* unit\_price).

Example: `6.9`

**unit\_price** `number` *required*

The unit price for this line item. Negative for a credit or deduction.

Example: `6.9`

**mailing\_address** `object | null` *required*

The billing/mailing address associated with this invoice, if one was provided at creation time.

**child attributes**

**city** `string | null` *required*

The city of the address.

**country** `string | null` *required*

The country of the address.

**line1** `string | null` *required*

The line 1 of the address.

**line2** `string | null` *required*

The line 2 of the address.

**name** `string | null` *required*

The name of the customer.

**phone** `string | null` *required*

The phone number of the customer.

**postal\_code** `string | null` *required*

The postal code of the address.

**state** `string | null` *required*

The state of the address.

**member** `object | null` *required*

The member that the invoice was created for. Null when the invoice is addressed to an email address with no member record behind it.

**child attributes**

**id** `string` *required*

The unique identifier for the company member.

**number** `string` *required*

The sequential invoice number for display purposes.

Example: `#0001`

**pay\_online\_url** `string | null` *required*

The checkout URL where the customer can pay this invoice online, with their
email address pre-filled and locked.

**payment** `object | null` *required*

The payment that settled this invoice. Null while the invoice is unpaid, when the invoice was marked paid manually, and on a subscription renewal invoice, where the settling payment cannot yet be identified.

**child attributes**

**id** `string` *required*

The unique identifier for the payment.

Example: `pay_xxxxxxxxxxxxxx`

**payment\_processing** `boolean` *required*

Whether a payment on this invoice is still clearing. True while a delayed
payment method such as ACH or SEPA settles, during which the invoice stays
open and is not marked past due.

**product** `object` *required*

The product that this invoice was generated for.

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`

**status** `InvoiceStatuses` *required*

The current payment status of the invoice, such as draft, open, paid, or void.

Available options: `draft`, `open`, `paid`, `past_due`, `uncollectible`, `void`

**subscription\_billing\_anchor\_at** `string\<date-time\> | null` *required*

The date that defines when the subscription billing cycle starts. When set on a renewal plan invoice, all future billing periods anchor to this date.

Example: `2023-12-01T05:00:00.401Z`

**updated\_at** `string\<date-time\>` *required*

The datetime the invoice was last updated.

Example: `2023-12-01T05:00:00.401Z`

**user** `object | null` *required*

The user this invoice is addressed to. Null if the user account has been removed.

**child attributes**

**email** `string | null` *required*

The user's email address. Requires the member:email:read permission to access. Null if not authorized.

Example: `john.doe@example.com`

**id** `string` *required*

The unique identifier for the user.

Example: `user_xxxxxxxxxxxxx`

**name** `string | null` *required*

The user's display name shown on their public profile.

Example: `Marcus Webb`

**username** `string` *required*

The user's unique username shown on their public profile.

Example: `marcuswebb`


## Related topics

- [Invoices](https://docs.whop.com/developer/guides/invoices.md)
- [Migrate from Legacy Invoices](https://docs.whop.com/developer/guides/migrate-from-legacy-invoices.md)
- [List Invoices](https://docs.whop.com/api-reference/invoices/list-invoices.md)
