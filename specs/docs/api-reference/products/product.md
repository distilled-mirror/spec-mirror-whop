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

# Products API

> A Product is a digital good or service sold on Whop. Products contain variants for pricing and may contain experiences for content delivery.

Use the Products API to search the public marketplace, list an account's products, retrieve a product, and create, update, or delete products.

> [!NOTE]
>
> Replaces the Legacy [Products](https://docs.whop.com/api-reference/products/product) resource.
> Existing Legacy integrations keep working; see [API
> versions](https://docs.whop.com/developer/api/versioning.md) for the stability contract.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Products](https://docs.whop.com/api-reference/products/list-products.md) | GET `/products` |
| [Retrieve Product](https://docs.whop.com/api-reference/products/retrieve-product.md) | GET `/products/{id}` |
| [Create Product](https://docs.whop.com/api-reference/products/create-product.md) | POST `/products` |
| [Publish Product](https://docs.whop.com/api-reference/products/publish-product.md) | POST `/products/{id}/publish` |
| [Unpublish Product](https://docs.whop.com/api-reference/products/unpublish-product.md) | POST `/products/{id}/unpublish` |
| [Update Product](https://docs.whop.com/api-reference/products/update-product.md) | PATCH `/products/{id}` |
| [Delete Product](https://docs.whop.com/api-reference/products/delete-product.md) | DELETE `/products/{id}` |

## Attributes

**id** `string` *required*

Product ID, prefixed `prod_`.

**account** `object | null` *required*

Account that sells this product.

**average\_review\_rating** `number` *required*

Average star rating across published reviews for this product, from `1.0` to
`5.0`. Returns `0.0` when no published-review rating is available.

**created\_at** `string` *required*

When the product was created, as an ISO 8601 timestamp.

**custom\_cta** `string | null` *required*

Call-to-action button label shown on the product purchase page.

Available options: `get_access`, `join`, `order_now`, `shop_now`, `call_now`, `donate_now`, `contact_us`, `sign_up`, `subscribe`, `purchase`, `get_offer`, `apply_now`, `complete_order`

**custom\_cta\_url** `string | null` *required*

URL the call-to-action button links to instead of checkout.

**custom\_statement\_descriptor** `string | null` *required*

Custom text label on customer's bank statement.

**default\_variant** `object | null` *required*

Buyable variant to show and check out with. The configured default when that variant is buyable, otherwise the first buyable variant in product-page order. `null` when none is buyable.

> [accordion] Properties
>
> **id** `string` *required*
>
> Variant ID, prefixed `plan_`.
>
> **billing\_period** `number | null` *required*
>
> Number of days between recurring charges, such as 30 for monthly or 365 for
> annual. `null` for one-time variants.
>
> **billing\_type** `string` *required*
>
> Billing model for this variant: `one_time` or `renewal`.
>
> Available options: `renewal`, `one_time`
>
> **expiration\_days** `number | null` *required*
>
> Access duration in days for expiration-based variants. `null` for variants
> without an expiration.
>
> **initial\_price** `object` *required*
>
> What checkout charges up front. `amount` is `"0.00"` when the first charge is free, such as a trial.
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
> **renewal\_price** `object` *required*
>
> The recurring charge every `billing_period` days. `amount` is `"0.00"` for one-time variants.
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
> **title** `string | null` *required*
>
> Variant display name shown to customers. `null` if no title has been set.
>
> **trial\_period\_days** `number | null` *required*
>
> Free trial days before the first renewal charge. `null` if no trial is
> configured or the signed-in user has already used a trial on this variant.
>
> **unlimited\_stock** `boolean` *required*
>
> Whether the variant has unlimited stock.
>
> **visibility** `string` *required*
>
> Where this variant can be seen. `visible` variants appear on the product page.
>
> Available options: `visible`, `hidden`, `archived`, `quick_link`

**description** `string | null` *required*

Written description displayed on the product page. `null` if none is set.

**experiences** `object[]` *required*

Experiences the product includes, such as courses, chats and apps, oldest first. Holds the first 100; page the rest through `GET /experiences?product_id=`.

> [accordion] Properties
>
> **id** `string` *required*
>
> Experience ID, prefixed `exp_`.
>
> **app** `object` *required*
>
> App the experience runs in.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > App ID, prefixed `app_`.
> >
> > **icon** `object` *required*
> >
> > App icon. Falls back to the default app icon when none is uploaded.
> >
> > > [accordion] Properties
> > >
> > > **url** `string` *required*
> > >
> > > Icon image URL. Always present — the default app icon when none is uploaded.
> >
> > **name** `string` *required*
> >
> > App display name.
>
> **logo\_url** `string | null` *required*
>
> Logo image URL uploaded for this experience. `null` when it has none; show the
> app's `icon` instead.
>
> **name** `string` *required*
>
> Experience display name.

**external\_identifier** `string | null` *required*

External identifier stored on the product for your own reference.

**gallery\_images** `object[]` *required*

Gallery images for this product, ordered by position.

> [accordion] Properties
>
> **id** `string` *required*
>
> Gallery image ID.
>
> **content\_type** `string | null` *required*
>
> Uploaded file MIME type, such as image/jpeg.
>
> **url** `string | null` *required*
>
> Pre-optimized URL for rendering this image on the client.

**global\_affiliate\_percentage** `number | null` *required*

Commission rate affiliates earn through the global affiliate program.

**global\_affiliate\_status** `string | null` *required*

Enrollment status in the global affiliate program.

Available options: `enabled`, `disabled`

**headline** `string | null` *required*

Short marketing headline displayed on product page.

**labels** `string[]` *required*

Lowercased labels used to group products into collections. Filter the list
endpoint by `labels` to fetch one collection.

**logo\_url** `string | null` *required*

Logo image URL shown on product pages and in search results. `null` when the
product has no logo.

**marketplace\_status** `string` *required*

Listing state on the whop.com marketplace. `pending_review` means submitted and awaiting review; `live_marketplace` means approved and discoverable.

Available options: `not_available`, `pending_review`, `live_marketplace`

**member\_affiliate\_percentage** `number | null` *required*

Commission rate members earn through the member affiliate program.

**member\_affiliate\_status** `string | null` *required*

Enrollment status in the member affiliate program.

Available options: `enabled`, `disabled`

**member\_count** `number` *required*

Active memberships for this product; 0 if public member counts are disabled.

**metadata** `object | null` *required*

Custom key-value pairs stored on the product.

**owner\_user** `object | null` *required*

User who owns the account selling this product.

**product\_tax\_code** `object | null` *required*

Tax classification code for this product, or `null` if no tax code is set.

**published\_reviews\_count** `number` *required*

Published customer reviews for this product.

**route** `string` *required*

URL slug for the product's public link.

**title** `string` *required*

Product display name shown to customers.

**updated\_at** `string` *required*

When the product was last updated, as an ISO 8601 timestamp.

**variant\_attributes** `object | null` *required*

The option set the product's variants span, as a map of attribute name to the values in use, e.g. `\{"color": ["Blue", "Red"], "size": ["S", "M", "L"]}`. Derived from the visible, non-invoice variants that carry `attributes`: keys alphabetical, values in the order the variants were created. Read-only. `null` when the product has no variants.

**variants** `object[] | null` *required*

The product's variants whose `visibility` is `visible` and that were not generated for an invoice, in creation order. Plain pricing options are included with `attributes: null`. Hidden, archived, quick-link, and invoice variants stay reachable through `GET /variants`. `null` when the product has more than 1000 variants (page them through `GET /variants?product_ids=`) and in webhook payloads.

> [accordion] Properties
>
> **id** `string` *required*
>
> Variant ID, prefixed `plan_`.
>
> **account** `object | null` *required*
>
> Account that sells this variant; `null` for standalone invoice variants.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Account ID, prefixed `biz_`.
> >
> > **title** `string` *required*
> >
> > Account display name.
>
> **adaptive\_pricing\_enabled** `boolean` *required*
>
> Whether adaptive pricing is enabled for this variant. Raw setting — does not
> check processor compatibility or feature flags.
>
> **allow\_multiple\_quantity** `boolean` *required*
>
> Whether more than one unit can be purchased in an order. Renewal variants
> cannot enable it. When `false`, checkouts, carts, and invoices reject a
> quantity above 1.
>
> **attributes** `object | null` *required*
>
> Attribute values that distinguish this variant within its product, as a map of attribute name to value, e.g. `\{"color": "Blue", "size": "Large"}`. Names are snake\_case identifiers and come back in alphabetical order. Every attributed variant on a product carries the same attribute names and a distinct set of values; the product lists the full option set as `variant_attributes`. `null` when the variant has no attributes.
>
> **billing\_period** `number | null` *required*
>
> Number of days between recurring charges, such as 30 for monthly or 365 for
> annual. `null` for one-time variants.
>
> **billing\_type** `string` *required*
>
> Billing model for this variant.
>
> Available options: `renewal`, `one_time`
>
> **cancel\_discount\_intervals** `number | null` *required*
>
> How long the cancellation discount lasts: `0` forever, `1` the next payment
> only, or a number of calendar months from when the customer accepts it. `null`
> when none is offered or the actor lacks the `plan:basic:read` scope.
>
> **cancel\_discount\_percentage** `number | null` *required*
>
> Cancellation discount as a whole-number percentage. `null` when none is
> offered or the actor lacks the `plan:basic:read` scope.
>
> **checkout\_styling** `object | null` *required*
>
> Variant-level checkout styling (`background_color`, `button_color`,
> `font_family`, `border_style`); `null` inherits the account default.
>
> **created\_at** `string` *required*
>
> When the variant was created, as an ISO 8601 timestamp.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 code of the currency every price on this variant is
> stated in, lowercase: the variant's own currency, or the
> `presentment_currency` the read asked for when the variant can be converted
> into it.
>
> **custom\_fields** `object[]` *required*
>
> Custom input fields collected on the checkout form.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Custom field ID, prefixed `field_`.
> >
> > **field\_type** `string` *required*
> >
> > Custom field input type.
> >
> > Available options: `text`
> >
> > **name** `string` *required*
> >
> > Field label shown to customer at checkout.
> >
> > **order** `number` *required*
> >
> > Field position on checkout form.
> >
> > **placeholder** `string | null` *required*
> >
> > Placeholder text shown in the empty field. `null` if none is set.
> >
> > **required** `boolean` *required*
> >
> > Whether the customer must complete this field to check out.
>
> **description** `string | null` *required*
>
> Customer-visible variant description. Maximum 1000 characters. `null` if no
> description is set.
>
> **expiration\_days** `number | null` *required*
>
> Access duration in days for expiration-based variants, such as 365 for a
> one-year pass. `null` for variants without an expiration.
>
> **formatted\_price** `string` *required*
>
> Human-readable price for display (currency + interval) in `currency`, e.g.
> "\$10 / month".
>
> **image** `object | null` *required*
>
> Pricing-tier image (`url`, `blurhash`) shown on the product page; `null` when
> no image is set.
>
> **initial\_price** `number` *required*
>
> Initial purchase price, in `currency`.
>
> **initial\_price\_due** `object` *required*
>
> Total charged at checkout for one unit, before promo codes and tax. For recurring variants it is `initial_price` plus the first `renewal_price`. It is `initial_price` alone while a free trial applies or the `subscription_starts_at` metadata key is in the future. The trial does not apply when the viewing user has already used one for this variant. Stated in `currency`.
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
> **internal\_notes** `string | null` *required*
>
> Private notes not shown to customers. `null` unless the actor has the
> `plan:basic:read` scope on the variant's account.
>
> **invoice** `object | null` *required*
>
> Invoice this variant was generated for; `null` unless created for an invoice.
>
> **member\_count** `number | null` *required*
>
> Active memberships through this variant. `null` unless the actor has the
> `plan:basic:read` scope on the variant's account.
>
> **metadata** `object | null` *required*
>
> Custom key-value pairs stored on the variant. Included in webhook payloads for
> payment and membership events. Maximum 50 keys, 100 characters per key, 500
> characters per value. The reserved keys `custom_cta` and `custom_cta_url`,
> when set, override the product's checkout call to action for this variant. The
> reserved key `subscription_starts_at` takes an ISO 8601 timestamp on a
> recurring variant. Until then, checkout saves the payment method without
> charging it. The membership is `scheduled` with no access until the first
> charge runs at that time.
>
> **offer\_cancel\_discount** `boolean | null` *required*
>
> Whether a cancellation discount is offered. `null` unless the actor has the
> `plan:basic:read` scope on the variant's account.
>
> **payment\_method\_configuration** `object | null` *required*
>
> Payment method configuration (`enabled`, `disabled`,
> `include_platform_defaults`); `null` when variant uses default settings.
>
> **product** `object | null` *required*
>
> Product this variant belongs to; `null` for standalone variants.
>
> **purchase\_url** `string` *required*
>
> URL where customers can purchase this variant directly.
>
> **redirect\_url** `string | null` *required*
>
> Where the customer is sent after paying for this variant. `null` when none is
> set, or unless the actor has the `plan:basic:read` scope on the variant's
> account.
>
> **release\_method** `string` *required*
>
> Sales method for this variant.
>
> Available options: `buy_now`, `waitlist`
>
> **renewal\_price** `number` *required*
>
> Recurring price charged every billing period, in `currency`.
>
> **sku** `string | null` *required*
>
> Stock keeping unit, free text set by the seller (e.g. `TSHIRT-LARGE-BLUE`).
> Not enforced unique. `null` when unset.
>
> **split\_pay\_required\_payments** `number | null` *required*
>
> Installment payments required before the subscription pauses. Must be greater
> than 1. `null` if split pay is not configured.
>
> **stock** `number | null` *required*
>
> Units available for purchase. `null` unless the actor has the
> `plan:basic:read` scope on the variant's account.
>
> **strike\_through\_initial\_price** `number | null` *required*
>
> Original initial price shown with a strikethrough, in `currency`. `null` when
> no strikethrough is set.
>
> **strike\_through\_renewal\_price** `number | null` *required*
>
> Original renewal price shown with a strikethrough, in `currency`. `null` when
> no strikethrough is set.
>
> **three\_ds\_level** `string | null` *required*
>
> 3D Secure behavior for supported on-session card payments. `mandate_challenge` requires a 3DS challenge before payment processing; `mandate_if_required` mandates a challenge only when the payment processor requires it; `frictionless_if_required` uses the regular frictionless 3DS flow. Payments of \$1,000 or more use `mandate_if_required` unless `mandate_challenge` is selected. Risk and authentication recovery requirements can override the preference. `null` inherits the account default.
>
> Available options: `mandate_challenge`, `mandate_if_required`, `frictionless_if_required`
>
> **title** `string | null` *required*
>
> Variant display name shown to customers. Maximum 30 characters. A variant
> created without one defaults to its attribute values joined with `/`. `null`
> if no title has been set.
>
> **trial\_period\_days** `number | null` *required*
>
> Free trial days before the first renewal charge. `null` if no trial is
> configured or the user has already used a trial for this variant.
>
> **unlimited\_stock** `boolean` *required*
>
> Whether the variant has unlimited stock. When `true`, the `stock` field is
> ignored; waitlist variants always report `true`.
>
> **updated\_at** `string` *required*
>
> When the variant was last updated, as an ISO 8601 timestamp.
>
> **visibility** `string` *required*
>
> Controls where this variant can be seen. When `hidden`, the variant is reachable only by its direct link.
>
> Available options: `visible`, `hidden`, `archived`, `quick_link`

**verified** `boolean` *required*

Whether the product has been verified by Whop.

**visibility** `string | null` *required*

Whether the product is publicly visible, hidden, or archived.

```json Product theme={null}
{
	"id": "prod_xxxxxxxxxxxxx",
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"route": "shine-time-auto-detailing",
		"title": "Shine Time Auto Detailing"
	},
	"average_review_rating": 4.75,
	"created_at": "2023-12-01T05:00:00.401Z",
	"custom_cta": "get_access",
	"custom_cta_url": "https://shinetime.example/book",
	"custom_statement_descriptor": "SHINE",
	"default_variant": {
		"id": "plan_xxxxxxxxxxxxx",
		"billing_period": 30,
		"billing_type": "renewal",
		"expiration_days": null,
		"initial_price": {
			"amount": "29.00",
			"currency": "usd",
			"decimals": 2,
			"display_decimals": 2
		},
		"renewal_price": {
			"amount": "29.00",
			"currency": "usd",
			"decimals": 2,
			"display_decimals": 2
		},
		"title": "Wash Club / 5 vehicles",
		"trial_period_days": 7,
		"unlimited_stock": true,
		"visibility": "visible"
	},
	"description": "Two-stage paint correction, a three-year ceramic coating, and sealed wheel faces.",
	"experiences": [
		{
			"id": "exp_xxxxxxxxxxxxxx",
			"app": {
				"id": "app_xxxxxxxxxxxxxx",
				"icon": {
					"url": "https://media.whop.com/abc123/icon.png"
				},
				"name": "Shine Time Booking"
			},
			"logo_url": null,
			"name": "Detailing bookings"
		}
	],
	"external_identifier": "ext_prod_12345",
	"gallery_images": [
		{
			"id": "file_xxxxxxxxxxxxx",
			"content_type": "image/jpeg",
			"url": "https://media.whop.com/abc123/optimized.jpg"
		}
	],
	"global_affiliate_percentage": 6.9,
	"global_affiliate_status": "enabled",
	"headline": "A showroom finish that lasts three years",
	"labels": ["detailing", "ceramic-coating"],
	"logo_url": "https://media.whop.com/abc123/logo.png",
	"marketplace_status": "live_marketplace",
	"member_affiliate_percentage": 6.9,
	"member_affiliate_status": "enabled",
	"member_count": 42,
	"metadata": {
		"external_product_id": "prod_123"
	},
	"owner_user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	},
	"product_tax_code": {
		"id": "ptc_xxxxxxxxxxxxxx",
		"name": "Services",
		"product_type": "services"
	},
	"published_reviews_count": 12,
	"route": "ceramic-coating-package",
	"title": "Ceramic Coating Package",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"variant_attributes": {
		"vehicle_count": ["5", "10"],
		"package": ["Wash Club", "Fleet Care"]
	},
	"variants": [
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
			"billing_type": "renewal",
			"cancel_discount_intervals": null,
			"cancel_discount_percentage": null,
			"checkout_styling": null,
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
			"description": "Monthly maintenance washes, interior vacuuming, and a quarterly sealant top-up.",
			"expiration_days": null,
			"formatted_price": "$29.00 / month",
			"image": null,
			"initial_price": 29,
			"initial_price_due": {
				"currency": "usd",
				"amount": "29.00",
				"decimals": 2,
				"display_decimals": 2
			},
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
			"product": {
				"id": "prod_xxxxxxxxxxxxx",
				"title": "Ceramic Coating Package"
			},
			"purchase_url": "https://whop.com/ceramic-coating-package/checkout/plan_xxxxxxxxxxxxx",
			"release_method": "buy_now",
			"renewal_price": 29,
			"sku": "SHINE-WASH-5-MONTHLY",
			"split_pay_required_payments": null,
			"stock": null,
			"strike_through_initial_price": null,
			"strike_through_renewal_price": null,
			"three_ds_level": "frictionless_if_required",
			"title": "Wash Club / 5 vehicles",
			"trial_period_days": 7,
			"unlimited_stock": true,
			"updated_at": "2023-12-01T05:00:00.401Z",
			"visibility": "visible"
		},
		{
			"id": "plan_yyyyyyyyyyyyy",
			"account": {
				"id": "biz_xxxxxxxxxxxxxx",
				"title": "Shine Time Auto Detailing"
			},
			"adaptive_pricing_enabled": true,
			"allow_multiple_quantity": false,
			"attributes": {
				"vehicle_count": "10",
				"package": "Wash Club"
			},
			"billing_period": 30,
			"billing_type": "renewal",
			"cancel_discount_intervals": null,
			"cancel_discount_percentage": null,
			"checkout_styling": null,
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
			"description": "Maintenance washes for up to 10 vehicles, billed monthly.",
			"expiration_days": null,
			"formatted_price": "$49.00 / month",
			"image": null,
			"initial_price": 49,
			"initial_price_due": {
				"currency": "usd",
				"amount": "49.00",
				"decimals": 2,
				"display_decimals": 2
			},
			"internal_notes": "Monthly maintenance wash membership",
			"redirect_url": null,
			"invoice": null,
			"member_count": 17,
			"metadata": {
				"external_plan_id": "pro-10"
			},
			"offer_cancel_discount": false,
			"payment_method_configuration": {
				"enabled": ["card"],
				"disabled": [],
				"include_platform_defaults": true
			},
			"product": {
				"id": "prod_xxxxxxxxxxxxx",
				"title": "Ceramic Coating Package"
			},
			"purchase_url": "https://whop.com/ceramic-coating-package/checkout/plan_yyyyyyyyyyyyy",
			"release_method": "buy_now",
			"renewal_price": 49,
			"sku": "SHINE-WASH-10-MONTHLY",
			"split_pay_required_payments": null,
			"stock": null,
			"strike_through_initial_price": null,
			"strike_through_renewal_price": null,
			"three_ds_level": "frictionless_if_required",
			"title": "Wash Club / 10 vehicles",
			"trial_period_days": 7,
			"unlimited_stock": true,
			"updated_at": "2023-12-01T05:00:00.401Z",
			"visibility": "visible"
		},
		{
			"id": "plan_zzzzzzzzzzzzz",
			"account": {
				"id": "biz_xxxxxxxxxxxxxx",
				"title": "Shine Time Auto Detailing"
			},
			"adaptive_pricing_enabled": true,
			"allow_multiple_quantity": false,
			"attributes": {
				"vehicle_count": "5",
				"package": "Fleet Care"
			},
			"billing_period": 30,
			"billing_type": "renewal",
			"cancel_discount_intervals": null,
			"cancel_discount_percentage": null,
			"checkout_styling": null,
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
			"description": "Fleet maintenance washes for up to 5 vehicles, billed monthly.",
			"expiration_days": null,
			"formatted_price": "$79.00 / month",
			"image": null,
			"initial_price": 79,
			"initial_price_due": {
				"currency": "usd",
				"amount": "79.00",
				"decimals": 2,
				"display_decimals": 2
			},
			"internal_notes": "Monthly maintenance wash membership",
			"redirect_url": null,
			"invoice": null,
			"member_count": 9,
			"metadata": {
				"external_plan_id": "team-5"
			},
			"offer_cancel_discount": false,
			"payment_method_configuration": {
				"enabled": ["card"],
				"disabled": [],
				"include_platform_defaults": true
			},
			"product": {
				"id": "prod_xxxxxxxxxxxxx",
				"title": "Ceramic Coating Package"
			},
			"purchase_url": "https://whop.com/ceramic-coating-package/checkout/plan_zzzzzzzzzzzzz",
			"release_method": "buy_now",
			"renewal_price": 79,
			"sku": "SHINE-FLEET-5-MONTHLY",
			"split_pay_required_payments": null,
			"stock": null,
			"strike_through_initial_price": null,
			"strike_through_renewal_price": null,
			"three_ds_level": "frictionless_if_required",
			"title": "Fleet Care / 5 vehicles",
			"trial_period_days": 7,
			"unlimited_stock": true,
			"updated_at": "2023-12-01T05:00:00.401Z",
			"visibility": "visible"
		},
		{
			"id": "plan_wwwwwwwwwwwww",
			"account": {
				"id": "biz_xxxxxxxxxxxxxx",
				"title": "Shine Time Auto Detailing"
			},
			"adaptive_pricing_enabled": true,
			"allow_multiple_quantity": false,
			"attributes": {
				"vehicle_count": "10",
				"package": "Fleet Care"
			},
			"billing_period": 30,
			"billing_type": "renewal",
			"cancel_discount_intervals": null,
			"cancel_discount_percentage": null,
			"checkout_styling": null,
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
			"description": "Fleet maintenance washes for up to 10 vehicles, billed monthly.",
			"expiration_days": null,
			"formatted_price": "$129.00 / month",
			"image": null,
			"initial_price": 129,
			"initial_price_due": {
				"currency": "usd",
				"amount": "129.00",
				"decimals": 2,
				"display_decimals": 2
			},
			"internal_notes": "Monthly maintenance wash membership",
			"redirect_url": null,
			"invoice": null,
			"member_count": 4,
			"metadata": {
				"external_plan_id": "team-10"
			},
			"offer_cancel_discount": false,
			"payment_method_configuration": {
				"enabled": ["card"],
				"disabled": [],
				"include_platform_defaults": true
			},
			"product": {
				"id": "prod_xxxxxxxxxxxxx",
				"title": "Ceramic Coating Package"
			},
			"purchase_url": "https://whop.com/ceramic-coating-package/checkout/plan_wwwwwwwwwwwww",
			"release_method": "buy_now",
			"renewal_price": 129,
			"sku": "SHINE-FLEET-10-MONTHLY",
			"split_pay_required_payments": null,
			"stock": null,
			"strike_through_initial_price": null,
			"strike_through_renewal_price": null,
			"three_ds_level": "frictionless_if_required",
			"title": "Fleet Care / 10 vehicles",
			"trial_period_days": 7,
			"unlimited_stock": true,
			"updated_at": "2023-12-01T05:00:00.401Z",
			"visibility": "visible"
		}
	],
	"verified": true,
	"visibility": "visible"
}
```


## Related topics

- [Product Affiliates API](https://docs.whop.com/api-reference/product-affiliates/product-affiliate.md)
- [Product](https://docs.whop.com/api-reference/legacy/products/product.md)
- [Delete product](https://docs.whop.com/api-reference/legacy/products/delete-product.md)
