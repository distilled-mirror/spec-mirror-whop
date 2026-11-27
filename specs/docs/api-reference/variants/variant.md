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

# Variants API

> A Variant is one purchasable configuration of a product.

It controls price, billing cadence, stock, SKU, attributes, tax behavior, checkout fields, and purchase visibility. Variant IDs remain prefixed `plan_`.

Use the Variants API to create variants for products, list existing variants, retrieve or update variant configuration, calculate tax for checkout, and delete variants that should no longer be offered.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Variants](https://docs.whop.com/api-reference/variants/list-variants.md) | GET `/variants` |
| [Retrieve Variant](https://docs.whop.com/api-reference/variants/retrieve-variant.md) | GET `/variants/{id}` |
| [Create Variant](https://docs.whop.com/api-reference/variants/create-variant.md) | POST `/variants` |
| [Update Variant](https://docs.whop.com/api-reference/variants/update-variant.md) | PATCH `/variants/{id}` |
| [Delete Variant](https://docs.whop.com/api-reference/variants/delete-variant.md) | DELETE `/variants/{id}` |

## Attributes

**id** `string` *required*

Variant ID, prefixed `plan_`.

**account** `object | null` *required*

Account that sells this variant; `null` for standalone invoice variants.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **title** `string` *required*
>
> Account display name.

**adaptive\_pricing\_enabled** `boolean` *required*

Whether adaptive pricing is enabled for this variant. Raw setting — does not
check processor compatibility or feature flags.

**allow\_multiple\_quantity** `boolean` *required*

Whether more than one unit can be purchased in an order. Renewal variants
cannot enable it. When `false`, checkouts, carts, and invoices reject a
quantity above 1.

**attributes** `object | null` *required*

Attribute values that distinguish this variant within its product, as a map of attribute name to value, e.g. `\{"color": "Blue", "size": "Large"}`. Names are snake\_case identifiers and come back in alphabetical order. Every attributed variant on a product carries the same attribute names and a distinct set of values; the product lists the full option set as `variant_attributes`. `null` when the variant has no attributes.

**billing\_period** `number | null` *required*

Number of days between recurring charges, such as 30 for monthly or 365 for
annual. `null` for one-time variants.

**billing\_type** `string` *required*

Billing model for this variant.

Available options: `renewal`, `one_time`

**cancel\_discount\_intervals** `number | null` *required*

How long the cancellation discount lasts: `0` forever, `1` the next payment
only, or a number of calendar months from when the customer accepts it. `null`
when none is offered or the actor lacks the `plan:basic:read` scope.

**cancel\_discount\_percentage** `number | null` *required*

Cancellation discount as a whole-number percentage. `null` when none is
offered or the actor lacks the `plan:basic:read` scope.

**checkout\_styling** `object | null` *required*

Variant-level checkout styling (`background_color`, `button_color`,
`font_family`, `border_style`); `null` inherits the account default.

**collect\_tax** `boolean` *required*

Whether tax is collected on purchases of this variant, based on the account's
tax configuration.

**created\_at** `string` *required*

When the variant was created, as an ISO 8601 timestamp.

**currency** `string` *required*

Three-letter ISO 4217 code of the currency every price on this variant is
stated in, lowercase: the variant's own currency, or the
`presentment_currency` the read asked for when the variant can be converted
into it.

**custom\_fields** `object[]` *required*

Custom input fields collected on the checkout form.

> [accordion] Properties
>
> **id** `string` *required*
>
> Custom field ID, prefixed `field_`.
>
> **field\_type** `string` *required*
>
> Custom field input type.
>
> Available options: `text`
>
> **name** `string` *required*
>
> Field label shown to customer at checkout.
>
> **order** `number` *required*
>
> Field position on checkout form.
>
> **placeholder** `string | null` *required*
>
> Placeholder text shown in the empty field. `null` if none is set.
>
> **required** `boolean` *required*
>
> Whether the customer must complete this field to check out.

**deletable** `boolean | null` *required*

Whether the variant can be deleted (it has no memberships or waitlist
entries). `null` unless the actor has the `plan:basic:read` scope on the
variant's account.

**description** `string | null` *required*

Customer-visible variant description. Maximum 1000 characters. `null` if no
description is set.

**effective\_payment\_method\_configuration** `object | null` *required*

The configuration governing a checkout for this variant, resolved through every layer (the variant's own and the account's) — the shape a session's `payment_method_configuration` carries. Apply it over the payment method types catalogue for the offerable set. `null` means platform defaults; `payment_method_configuration` stays the variant's own editable override.

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

**expiration\_days** `number | null` *required*

Access duration in days for expiration-based variants, such as 365 for a
one-year pass. `null` for variants without an expiration.

**formatted\_price** `string` *required*

Human-readable price for display (currency + interval) in `currency`, e.g.
"\$10 / month".

**image** `object | null` *required*

Pricing-tier image (`url`, `blurhash`) shown on the product page; `null` when
no image is set.

**initial\_price** `number` *required*

Initial purchase price, in `currency`.

**initial\_price\_due** `object` *required*

Total charged at checkout for one unit, before promo codes and tax. For recurring variants it is `initial_price` plus the first `renewal_price`. It is `initial_price` alone while a free trial applies or the `subscription_starts_at` metadata key is in the future. The trial does not apply when the viewing user has already used one for this variant. Stated in `currency`.

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

**internal\_notes** `string | null` *required*

Private notes not shown to customers. `null` unless the actor has the
`plan:basic:read` scope on the variant's account.

**invoice** `object | null` *required*

Invoice this variant was generated for; `null` unless created for an invoice.

**member\_count** `number | null` *required*

Active memberships through this variant. `null` unless the actor has the
`plan:basic:read` scope on the variant's account.

**metadata** `object | null` *required*

Custom key-value pairs stored on the variant. Included in webhook payloads for
payment and membership events. Maximum 50 keys, 100 characters per key, 500
characters per value. The reserved keys `custom_cta` and `custom_cta_url`,
when set, override the product's checkout call to action for this variant. The
reserved key `subscription_starts_at` takes an ISO 8601 timestamp on a
recurring variant. Until then, checkout saves the payment method without
charging it. The membership is `scheduled` with no access until the first
charge runs at that time.

**offer\_cancel\_discount** `boolean | null` *required*

Whether a cancellation discount is offered. `null` unless the actor has the
`plan:basic:read` scope on the variant's account.

**payment\_method\_configuration** `object | null` *required*

Payment method configuration (`enabled`, `disabled`,
`include_platform_defaults`); `null` when variant uses default settings.

**product** `object | null` *required*

Product this variant belongs to; `null` for standalone variants.

**purchase\_url** `string` *required*

URL where customers can purchase this variant directly.

**redirect\_url** `string | null` *required*

Where the customer is sent after paying for this variant. `null` when none is
set, or unless the actor has the `plan:basic:read` scope on the variant's
account.

**release\_method** `string` *required*

Sales method for this variant.

Available options: `buy_now`, `waitlist`

**renewal\_price** `number` *required*

Recurring price charged every billing period, in `currency`.

**sku** `string | null` *required*

Stock keeping unit, free text set by the seller (e.g. `TSHIRT-LARGE-BLUE`).
Not enforced unique. `null` when unset.

**split\_pay\_required\_payments** `number | null` *required*

Installment payments required before the subscription pauses. Must be greater
than 1. `null` if split pay is not configured.

**stock** `number | null` *required*

Units available for purchase. `null` unless the actor has the
`plan:basic:read` scope on the variant's account.

**strike\_through\_initial\_price** `number | null` *required*

Original initial price shown with a strikethrough, in `currency`. `null` when
no strikethrough is set.

**strike\_through\_renewal\_price** `number | null` *required*

Original renewal price shown with a strikethrough, in `currency`. `null` when
no strikethrough is set.

**tax\_type** `string` *required*

How tax is handled for this variant, including whether tax is included in the price, added at checkout, or not configured.

Available options: `inclusive`, `exclusive`, `unspecified`

**three\_ds\_level** `string | null` *required*

3D Secure behavior for supported on-session card payments. `mandate_challenge` requires a 3DS challenge before payment processing; `mandate_if_required` mandates a challenge only when the payment processor requires it; `frictionless_if_required` uses the regular frictionless 3DS flow. Payments of \$1,000 or more use `mandate_if_required` unless `mandate_challenge` is selected. Risk and authentication recovery requirements can override the preference. `null` inherits the account default.

Available options: `mandate_challenge`, `mandate_if_required`, `frictionless_if_required`

**title** `string | null` *required*

Variant display name shown to customers. Maximum 30 characters. A variant
created without one defaults to its attribute values joined with `/`. `null`
if no title has been set.

**trial\_period\_days** `number | null` *required*

Free trial days before the first renewal charge. `null` if no trial is
configured or the user has already used a trial for this variant.

**unlimited\_stock** `boolean` *required*

Whether the variant has unlimited stock. When `true`, the `stock` field is
ignored; waitlist variants always report `true`.

**unpaid\_invoice\_count** `integer | null` *required*

Number of issued invoices for this variant that are not paid or void. While it
is above `0`, you can't change the variant's prices, billing period, trial,
number of payments, currency or billing type. `null` unless the actor has the
`plan:basic:read` scope on the variant's account.

**updated\_at** `string` *required*

When the variant was last updated, as an ISO 8601 timestamp.

**visibility** `string` *required*

Controls where this variant can be seen. When `hidden`, the variant is reachable only by its direct link.

Available options: `visible`, `hidden`, `archived`, `quick_link`

```json Variant theme={null}
{
	"id": "plan_xxxxxxxxxxxxx",
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"adaptive_pricing_enabled": true,
	"allow_multiple_quantity": false,
	"attributes": {
		"vehicle_count": "5",
		"package": "Wash Club"
	},
	"billing_period": 30,
	"cancel_discount_intervals": null,
	"cancel_discount_percentage": null,
	"checkout_styling": null,
	"collect_tax": true,
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "usd",
	"custom_fields": [
		{
			"id": "cusf_xxxxxxxxxxxx",
			"field_type": "text",
			"name": "Vehicle make and model",
			"order": 0,
			"placeholder": "e.g. Toyota Tacoma",
			"required": true
		}
	],
	"deletable": false,
	"description": "Monthly maintenance washes, interior vacuuming, and a quarterly sealant top-up.",
	"expiration_days": null,
	"formatted_price": "$29.00 / month",
	"image": null,
	"initial_price": 29,
	"internal_notes": "Monthly maintenance wash membership",
	"redirect_url": null,
	"invoice": null,
	"member_count": 42,
	"metadata": {
		"external_plan_id": "monthly",
		"custom_cta": "subscribe",
		"custom_cta_url": "https://shinetime.example/wash-club"
	},
	"offer_cancel_discount": false,
	"payment_method_configuration": {
		"enabled": ["card"],
		"disabled": [],
		"include_platform_defaults": true
	},
	"effective_payment_method_configuration": {
		"enabled": ["card"],
		"disabled": [],
		"include_platform_defaults": true
	},
	"billing_type": "renewal",
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	},
	"purchase_url": "https://whop.com/ceramic-coating-package/checkout/plan_xxxxxxxxxxxxx",
	"release_method": "buy_now",
	"renewal_price": 29,
	"initial_price_due": {
		"currency": "usd",
		"amount": "29.00",
		"decimals": 2,
		"display_decimals": 2
	},
	"sku": "SHINE-WASH-5-MONTHLY",
	"split_pay_required_payments": null,
	"stock": null,
	"strike_through_initial_price": null,
	"strike_through_renewal_price": null,
	"tax_type": "exclusive",
	"three_ds_level": "frictionless_if_required",
	"title": "Wash Club / 5 vehicles",
	"trial_period_days": 7,
	"unlimited_stock": true,
	"unpaid_invoice_count": 0,
	"updated_at": "2023-12-01T05:00:00.401Z",
	"visibility": "visible"
}
```


## Related topics

- [Variants](https://docs.whop.com/api-reference/legacy/variants/variant.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Delete Variant](https://docs.whop.com/api-reference/variants/delete-variant.md)
