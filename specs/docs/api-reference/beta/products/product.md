> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Product

A Product is a digital good or service sold on Whop. Products may contain plans for pricing and/or experiences for content delivery.

Use the Products API to search the public marketplace, list an account's products, retrieve a product, and create, update, or delete products.

<Note>
  Replaces the Legacy [Products](/api-reference/products/product) resource.
  Existing Legacy integrations keep working; see [API
  versions](/developer/api/versioning) for the stability contract.
</Note>

## Endpoints

| Endpoint                                                            | Request                                                                       |
| ------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| [List Products](/api-reference/beta/products/list-products)         | <Badge color="blue" size="sm" stroke>GET</Badge> `/products`                  |
| [Retrieve Product](/api-reference/beta/products/retrieve-product)   | <Badge color="blue" size="sm" stroke>GET</Badge> `/products/{id}`             |
| [Create Product](/api-reference/beta/products/create-product)       | <Badge color="green" size="sm" stroke>POST</Badge> `/products`                |
| [Publish Product](/api-reference/beta/products/publish-product)     | <Badge color="green" size="sm" stroke>POST</Badge> `/products/{id}/publish`   |
| [Unpublish Product](/api-reference/beta/products/unpublish-product) | <Badge color="green" size="sm" stroke>POST</Badge> `/products/{id}/unpublish` |
| [Update Product](/api-reference/beta/products/update-product)       | <Badge color="orange" size="sm" stroke>PATCH</Badge> `/products/{id}`         |
| [Delete Product](/api-reference/beta/products/delete-product)       | <Badge color="red" size="sm" stroke>DELETE</Badge> `/products/{id}`           |

## Attributes

<Columns cols={2}>
  <Column>
    <ResponseField name="id" type="string" required>
      Product ID, prefixed `prod_`.
    </ResponseField>

    <ResponseField name="account" type="object | null" required>
      Account that sells this product.
    </ResponseField>

    <ResponseField name="average_review_rating" type="number" required>
      Average star rating across published reviews for this product, from `1.0` to
      `5.0`. Returns `0.0` when no published-review rating is available.
    </ResponseField>

    <ResponseField name="created_at" type="string" required>
      When the product was created, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="custom_cta" type="string | null" required>
      Call-to-action button label shown on the product purchase page.

      Available options: `get_access`, `join`, `order_now`, `shop_now`, `call_now`, `donate_now`, `contact_us`, `sign_up`, `subscribe`, `purchase`, `get_offer`, `apply_now`, `complete_order`
    </ResponseField>

    <ResponseField name="custom_cta_url" type="string | null" required>
      URL the call-to-action button links to instead of checkout.
    </ResponseField>

    <ResponseField name="custom_statement_descriptor" type="string | null" required>
      Custom text label on customer's bank statement.
    </ResponseField>

    <ResponseField name="default_plan" type="object | null" required>
      Buyable plan to show and check out with. The configured default when that plan is buyable, otherwise the first buyable plan in product-page order. `null` when none is buyable.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Plan ID, prefixed `plan_`.
        </ResponseField>

        <ResponseField name="billing_period" type="number | null" required>
          Number of days between recurring charges, such as 30 for monthly or 365 for
          annual. `null` for one-time plans.
        </ResponseField>

        <ResponseField name="expiration_days" type="number | null" required>
          Access duration in days for expiration-based plans. `null` for plans without
          an expiration.
        </ResponseField>

        <ResponseField name="initial_price" type="object" required>
          What checkout charges up front. `amount` is `"0.00"` when the first charge is free, such as a trial.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="amount" type="string" required>
              The amount in major units, as an exact decimal string — `"10.00"` is ten
              dollars. A string so no float rounds it in transit.
            </ResponseField>

            <ResponseField name="currency" type="string" required>
              Three-letter ISO 4217 currency code, lowercase.
            </ResponseField>

            <ResponseField name="decimals" type="integer" required>
              How many decimal places the amount CARRIES — the precision the charge itself
              runs at.
            </ResponseField>

            <ResponseField name="display_decimals" type="integer" required>
              How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="plan_type" type="string" required>
          Billing model for this plan: `one_time` or `renewal`.

          Available options: `renewal`, `one_time`
        </ResponseField>

        <ResponseField name="renewal_price" type="object" required>
          The recurring charge every `billing_period` days. `amount` is `"0.00"` for one-time plans.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="amount" type="string" required>
              The amount in major units, as an exact decimal string — `"10.00"` is ten
              dollars. A string so no float rounds it in transit.
            </ResponseField>

            <ResponseField name="currency" type="string" required>
              Three-letter ISO 4217 currency code, lowercase.
            </ResponseField>

            <ResponseField name="decimals" type="integer" required>
              How many decimal places the amount CARRIES — the precision the charge itself
              runs at.
            </ResponseField>

            <ResponseField name="display_decimals" type="integer" required>
              How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="title" type="string | null" required>
          Plan display name shown to customers. `null` if no title has been set.
        </ResponseField>

        <ResponseField name="unlimited_stock" type="boolean" required>
          Whether the plan has unlimited stock.
        </ResponseField>

        <ResponseField name="visibility" type="string" required>
          Where this plan can be seen. `visible` plans appear on the product page.

          Available options: `visible`, `hidden`, `archived`, `quick_link`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="description" type="string | null" required>
      Written description displayed on the product page. `null` if none is set.
    </ResponseField>

    <ResponseField name="external_identifier" type="string | null" required>
      External identifier stored on the product for your own reference.
    </ResponseField>

    <ResponseField name="gallery_images" type="object[]" required>
      Gallery images for this product, ordered by position.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Gallery image ID.
        </ResponseField>

        <ResponseField name="content_type" type="string | null" required>
          Uploaded file MIME type, such as image/jpeg.
        </ResponseField>

        <ResponseField name="url" type="string | null" required>
          Pre-optimized URL for rendering this image on the client.
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="global_affiliate_percentage" type="number | null" required>
      Commission rate affiliates earn through the global affiliate program.
    </ResponseField>

    <ResponseField name="global_affiliate_status" type="string | null" required>
      Enrollment status in the global affiliate program.

      Available options: `enabled`, `disabled`
    </ResponseField>

    <ResponseField name="headline" type="string | null" required>
      Short marketing headline displayed on product page.
    </ResponseField>

    <ResponseField name="labels" type="string[]" required>
      Lowercased labels used to group products into collections. Filter the list
      endpoint by `labels` to fetch one collection.
    </ResponseField>

    <ResponseField name="marketplace_status" type="string" required>
      Listing state on the whop.com marketplace. `pending_review` means submitted and awaiting review; `live_marketplace` means approved and discoverable.

      Available options: `not_available`, `pending_review`, `live_marketplace`
    </ResponseField>

    <ResponseField name="member_affiliate_percentage" type="number | null" required>
      Commission rate members earn through the member affiliate program.
    </ResponseField>

    <ResponseField name="member_affiliate_status" type="string | null" required>
      Enrollment status in the member affiliate program.

      Available options: `enabled`, `disabled`
    </ResponseField>

    <ResponseField name="member_count" type="number" required>
      Active memberships for this product; 0 if public member counts are disabled.
    </ResponseField>

    <ResponseField name="metadata" type="object | null" required>
      Custom key-value pairs stored on the product.
    </ResponseField>

    <ResponseField name="owner_user" type="object | null" required>
      User who owns the account selling this product.
    </ResponseField>

    <ResponseField name="product_tax_code" type="object | null" required>
      Tax classification code for this product, or `null` if no tax code is set.
    </ResponseField>

    <ResponseField name="published_reviews_count" type="number" required>
      Published customer reviews for this product.
    </ResponseField>

    <ResponseField name="route" type="string" required>
      URL slug for the product's public link.
    </ResponseField>

    <ResponseField name="title" type="string" required>
      Product display name shown to customers.
    </ResponseField>

    <ResponseField name="updated_at" type="string" required>
      When the product was last updated, as an ISO 8601 timestamp.
    </ResponseField>

    <ResponseField name="variant_attributes" type="object | null" required>
      The option set the product's variants span, as a map of attribute name to the values in use, e.g. `\{"color": ["Blue", "Red"], "size": ["S", "M", "L"]}`. Derived from the visible, non-invoice plans that carry `attributes`: keys alphabetical, values in the order the plans were created. Read-only. `null` when the product has no variants.
    </ResponseField>

    <ResponseField name="variants" type="object[] | null" required>
      The product's plans whose `visibility` is `visible` and that were not generated for an invoice, in creation order, plain pricing options included with `attributes: null`, serialized as `GET /plans` list items. Hidden, archived, quick-link and invoice plans stay reachable through `GET /plans`. `null` when the product has more than 1000 such plans (page them through `GET /plans?product_ids=`) and in webhook payloads.

      <Accordion title="Properties" defaultOpen={true}>
        <ResponseField name="id" type="string" required>
          Plan ID, prefixed `plan_`.
        </ResponseField>

        <ResponseField name="account" type="object | null" required>
          Account that sells this plan; `null` for standalone invoice plans.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="id" type="string" required>
              Account ID, prefixed `biz_`.
            </ResponseField>

            <ResponseField name="title" type="string" required>
              Account display name.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="adaptive_pricing_enabled" type="boolean" required>
          Whether adaptive pricing is enabled for this plan. Raw setting — does not
          check processor compatibility or feature flags.
        </ResponseField>

        <ResponseField name="attributes" type="object | null" required>
          Attribute values that make this plan one variant of its product, as a map of attribute name to value, e.g. `\{"color": "Blue", "size": "Large"}`. Names are snake\_case identifiers and come back in alphabetical order. Every variant plan on a product carries the same attribute names and a distinct set of values; the product lists the full option set as `variant_attributes`. `null` for a plan that is not a variant.
        </ResponseField>

        <ResponseField name="billing_period" type="number | null" required>
          Number of days between recurring charges, such as 30 for monthly or 365 for
          annual. `null` for one-time plans.
        </ResponseField>

        <ResponseField name="cancel_discount_intervals" type="number | null" required>
          Billing intervals the cancellation discount applies to (`0` forever, `1` first
          payment, or a month count). `null` when none is offered or the actor lacks the
          `plan:basic:read` scope.
        </ResponseField>

        <ResponseField name="cancel_discount_percentage" type="number | null" required>
          Cancellation discount as a whole-number percentage. `null` when none is
          offered or the actor lacks the `plan:basic:read` scope.
        </ResponseField>

        <ResponseField name="checkout_styling" type="object | null" required>
          Plan-level checkout styling (`background_color`, `button_color`,
          `font_family`, `border_style`); `null` inherits the account default.
        </ResponseField>

        <ResponseField name="created_at" type="string" required>
          When the plan was created, as an ISO 8601 timestamp.
        </ResponseField>

        <ResponseField name="currency" type="string" required>
          Three-letter ISO currency code for this plan's prices.
        </ResponseField>

        <ResponseField name="custom_fields" type="object[]" required>
          Custom input fields collected on the checkout form.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="id" type="string" required>
              Custom field ID, prefixed `field_`.
            </ResponseField>

            <ResponseField name="field_type" type="string" required>
              Custom field input type.

              Available options: `text`
            </ResponseField>

            <ResponseField name="name" type="string" required>
              Field label shown to customer at checkout.
            </ResponseField>

            <ResponseField name="order" type="number" required>
              Field position on checkout form.
            </ResponseField>

            <ResponseField name="placeholder" type="string | null" required>
              Placeholder text shown in the empty field. `null` if none is set.
            </ResponseField>

            <ResponseField name="required" type="boolean" required>
              Whether the customer must complete this field to check out.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="description" type="string | null" required>
          Customer-visible plan description. Maximum 1000 characters. `null` if no
          description is set.
        </ResponseField>

        <ResponseField name="expiration_days" type="number | null" required>
          Access duration in days for expiration-based plans, such as 365 for a one-year
          pass. `null` for plans without an expiration.
        </ResponseField>

        <ResponseField name="formatted_price" type="string" required>
          Human-readable price for display (currency + interval), e.g. "\$10 / month".
        </ResponseField>

        <ResponseField name="image" type="object | null" required>
          Pricing-tier image (`url`, `blurhash`) shown on the product page; `null` when
          no image is set.
        </ResponseField>

        <ResponseField name="initial_price" type="number" required>
          Initial purchase price in plan currency.
        </ResponseField>

        <ResponseField name="initial_price_due" type="object" required>
          Total charged at checkout for one unit, before promo codes and tax: `initial_price` plus the first `renewal_price` for recurring plans, or `initial_price` alone while a free trial applies. The trial does not apply when the viewing user has already used one for this plan.

          <Accordion title="Properties" defaultOpen={true}>
            <ResponseField name="amount" type="string" required>
              The amount in major units, as an exact decimal string — `"10.00"` is ten
              dollars. A string so no float rounds it in transit.
            </ResponseField>

            <ResponseField name="currency" type="string" required>
              Three-letter ISO 4217 currency code, lowercase.
            </ResponseField>

            <ResponseField name="decimals" type="integer" required>
              How many decimal places the amount CARRIES — the precision the charge itself
              runs at.
            </ResponseField>

            <ResponseField name="display_decimals" type="integer" required>
              How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
            </ResponseField>
          </Accordion>
        </ResponseField>

        <ResponseField name="internal_notes" type="string | null" required>
          Private notes not shown to customers. `null` unless the actor has the
          `plan:basic:read` scope on the plan's account.
        </ResponseField>

        <ResponseField name="invoice" type="object | null" required>
          Invoice this plan was generated for; `null` unless created for an invoice.
        </ResponseField>

        <ResponseField name="member_count" type="number | null" required>
          Active memberships through this plan. `null` unless the actor has the
          `plan:basic:read` scope on the plan's account.
        </ResponseField>

        <ResponseField name="metadata" type="object | null" required>
          Custom key-value pairs stored on the plan. Included in webhook payloads for
          payment and membership events. Maximum 50 keys, 100 characters per key, 500
          characters per value. The reserved keys `custom_cta` and `custom_cta_url`,
          when set, override the product's checkout call to action for this plan.
        </ResponseField>

        <ResponseField name="offer_cancel_discount" type="boolean | null" required>
          Whether a cancellation discount is offered. `null` unless the actor has the
          `plan:basic:read` scope on the plan's account.
        </ResponseField>

        <ResponseField name="payment_method_configuration" type="object | null" required>
          Payment method configuration (`enabled`, `disabled`,
          `include_platform_defaults`); `null` when plan uses default settings.
        </ResponseField>

        <ResponseField name="plan_type" type="string" required>
          Billing model for this plan.

          Available options: `renewal`, `one_time`
        </ResponseField>

        <ResponseField name="product" type="object | null" required>
          Product this plan belongs to; `null` for standalone plans.
        </ResponseField>

        <ResponseField name="purchase_url" type="string" required>
          URL where customers can purchase this plan directly.
        </ResponseField>

        <ResponseField name="release_method" type="string" required>
          Sales method for this plan.

          Available options: `buy_now`, `waitlist`
        </ResponseField>

        <ResponseField name="renewal_price" type="number" required>
          Recurring price charged every billing period.
        </ResponseField>

        <ResponseField name="sku" type="string | null" required>
          Stock keeping unit, free text set by the seller (e.g. `TSHIRT-LARGE-BLUE`).
          Not enforced unique. `null` when unset.
        </ResponseField>

        <ResponseField name="split_pay_required_payments" type="number | null" required>
          Installment payments required before the subscription pauses. Must be greater
          than 1. `null` if split pay is not configured.
        </ResponseField>

        <ResponseField name="stock" type="number | null" required>
          Units available for purchase. `null` unless the actor has the
          `plan:basic:read` scope on the plan's account.
        </ResponseField>

        <ResponseField name="strike_through_initial_price" type="number | null" required>
          Original initial price shown with a strikethrough, in the plan's currency.
          `null` when no strikethrough is set.
        </ResponseField>

        <ResponseField name="strike_through_renewal_price" type="number | null" required>
          Original renewal price shown with a strikethrough, in the plan's currency.
          `null` when no strikethrough is set.
        </ResponseField>

        <ResponseField name="three_ds_level" type="string | null" required>
          3D Secure behavior for supported on-session card payments. `mandate_challenge` requires a 3DS challenge before payment processing; `mandate_if_required` mandates a challenge only when the payment processor requires it; `frictionless_if_required` uses the regular frictionless 3DS flow. Payments of \$1,000 or more use `mandate_if_required` unless `mandate_challenge` is selected. Risk and authentication recovery requirements can override the preference. `null` inherits the account default.

          Available options: `mandate_challenge`, `mandate_if_required`, `frictionless_if_required`
        </ResponseField>

        <ResponseField name="title" type="string | null" required>
          Plan display name shown to customers. Maximum 30 characters. A variant created
          without one defaults to its attribute values joined with `/`. `null` if no
          title has been set.
        </ResponseField>

        <ResponseField name="trial_period_days" type="number | null" required>
          Free trial days before the first renewal charge. `null` if no trial is
          configured or the user has already used a trial for this plan.
        </ResponseField>

        <ResponseField name="unlimited_stock" type="boolean" required>
          Whether the plan has unlimited stock. When `true`, the `stock` field is
          ignored; waitlist plans always report `true`.
        </ResponseField>

        <ResponseField name="updated_at" type="string" required>
          When the plan was last updated, as an ISO 8601 timestamp.
        </ResponseField>

        <ResponseField name="visibility" type="string" required>
          Controls where this plan can be seen. When `hidden`, the plan is reachable only by its direct link.

          Available options: `visible`, `hidden`, `archived`, `quick_link`
        </ResponseField>
      </Accordion>
    </ResponseField>

    <ResponseField name="verified" type="boolean" required>
      Whether the product has been verified by Whop.
    </ResponseField>

    <ResponseField name="visibility" type="string | null" required>
      Whether the product is publicly visible, hidden, or archived.
    </ResponseField>
  </Column>

  <Column>
    <div className="api-resource-sticky-example">
      ```json Product theme={null}
      {
      	"id": "prod_xxxxxxxxxxxxx",
      	"account": {
      		"id": "biz_xxxxxxxxxxxxxx",
      		"route": "pickaxe",
      		"title": "Pickaxe"
      	},
      	"average_review_rating": 4.75,
      	"created_at": "2023-12-01T05:00:00.401Z",
      	"custom_cta": "get_access",
      	"custom_cta_url": "https://example.com/signup",
      	"custom_statement_descriptor": "PICKAXE",
      	"default_plan": {
      		"id": "plan_xxxxxxxxxxxxx",
      		"billing_period": 30,
      		"expiration_days": null,
      		"initial_price": {
      			"amount": "29.00",
      			"currency": "usd",
      			"decimals": 2,
      			"display_decimals": 2
      		},
      		"plan_type": "renewal",
      		"renewal_price": {
      			"amount": "29.00",
      			"currency": "usd",
      			"decimals": 2,
      			"display_decimals": 2
      		},
      		"title": "Pro / 5 seats",
      		"unlimited_stock": true,
      		"visibility": "visible"
      	},
      	"description": "Track your revenue, members, and growth in real time.",
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
      	"headline": "Real-time data analytics for creators",
      	"labels": ["analytics", "creator-tools"],
      	"marketplace_status": "live_marketplace",
      	"member_affiliate_percentage": 6.9,
      	"member_affiliate_status": "enabled",
      	"member_count": 42,
      	"metadata": {
      		"external_product_id": "prod_123"
      	},
      	"owner_user": {
      		"id": "user_xxxxxxxxxxxxx",
      		"name": "John Doe",
      		"username": "johndoe42"
      	},
      	"product_tax_code": {
      		"id": "ptc_xxxxxxxxxxxxxx",
      		"name": "Digital - SaaS",
      		"product_type": "digital"
      	},
      	"published_reviews_count": 12,
      	"route": "pickaxe-analytics",
      	"title": "Pickaxe Analytics",
      	"updated_at": "2023-12-01T05:00:00.401Z",
      	"variant_attributes": {
      		"seats": ["5", "10"],
      		"tier": ["Pro", "Team"]
      	},
      	"variants": [
      		{
      			"id": "plan_xxxxxxxxxxxxx",
      			"account": {
      				"id": "biz_xxxxxxxxxxxxxx",
      				"title": "Pickaxe"
      			},
      			"adaptive_pricing_enabled": true,
      			"attributes": {
      				"seats": "5",
      				"tier": "Pro"
      			},
      			"billing_period": 30,
      			"cancel_discount_intervals": null,
      			"cancel_discount_percentage": null,
      			"checkout_styling": null,
      			"created_at": "2023-12-01T05:00:00.401Z",
      			"currency": "usd",
      			"custom_fields": [
      				{
      					"id": "cusf_xxxxxxxxxxxx",
      					"field_type": "text",
      					"name": "Discord username",
      					"order": 0,
      					"placeholder": "e.g. pickaxe_user",
      					"required": true
      				}
      			],
      			"description": "Monthly access to Pickaxe Analytics.",
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
      			"internal_notes": "Standard monthly plan",
      			"invoice": null,
      			"member_count": 42,
      			"metadata": {
      				"external_plan_id": "monthly",
      				"custom_cta": "subscribe",
      				"custom_cta_url": "https://example.com/wash-club"
      			},
      			"offer_cancel_discount": false,
      			"payment_method_configuration": {
      				"enabled": ["card"],
      				"disabled": [],
      				"include_platform_defaults": true
      			},
      			"plan_type": "renewal",
      			"product": {
      				"id": "prod_xxxxxxxxxxxxx",
      				"title": "Pickaxe Analytics"
      			},
      			"purchase_url": "https://whop.com/pickaxe-analytics/checkout/plan_xxxxxxxxxxxxx",
      			"release_method": "buy_now",
      			"renewal_price": 29,
      			"sku": "PICKAXE-PRO-5-MONTHLY",
      			"split_pay_required_payments": null,
      			"stock": null,
      			"strike_through_initial_price": null,
      			"strike_through_renewal_price": null,
      			"three_ds_level": "frictionless_if_required",
      			"title": "Pro / 5 seats",
      			"trial_period_days": 7,
      			"unlimited_stock": true,
      			"updated_at": "2023-12-01T05:00:00.401Z",
      			"visibility": "visible"
      		},
      		{
      			"id": "plan_yyyyyyyyyyyyy",
      			"account": {
      				"id": "biz_xxxxxxxxxxxxxx",
      				"title": "Pickaxe"
      			},
      			"adaptive_pricing_enabled": true,
      			"attributes": {
      				"seats": "10",
      				"tier": "Pro"
      			},
      			"billing_period": 30,
      			"cancel_discount_intervals": null,
      			"cancel_discount_percentage": null,
      			"checkout_styling": null,
      			"created_at": "2023-12-01T05:00:00.401Z",
      			"currency": "usd",
      			"custom_fields": [
      				{
      					"id": "cusf_xxxxxxxxxxxx",
      					"field_type": "text",
      					"name": "Discord username",
      					"order": 0,
      					"placeholder": "e.g. pickaxe_user",
      					"required": true
      				}
      			],
      			"description": "Pro tier for up to 10 seats, billed monthly.",
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
      			"internal_notes": "Standard monthly plan",
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
      			"plan_type": "renewal",
      			"product": {
      				"id": "prod_xxxxxxxxxxxxx",
      				"title": "Pickaxe Analytics"
      			},
      			"purchase_url": "https://whop.com/pickaxe-analytics/checkout/plan_yyyyyyyyyyyyy",
      			"release_method": "buy_now",
      			"renewal_price": 49,
      			"sku": "PICKAXE-PRO-10-MONTHLY",
      			"split_pay_required_payments": null,
      			"stock": null,
      			"strike_through_initial_price": null,
      			"strike_through_renewal_price": null,
      			"three_ds_level": "frictionless_if_required",
      			"title": "Pro / 10 seats",
      			"trial_period_days": 7,
      			"unlimited_stock": true,
      			"updated_at": "2023-12-01T05:00:00.401Z",
      			"visibility": "visible"
      		},
      		{
      			"id": "plan_zzzzzzzzzzzzz",
      			"account": {
      				"id": "biz_xxxxxxxxxxxxxx",
      				"title": "Pickaxe"
      			},
      			"adaptive_pricing_enabled": true,
      			"attributes": {
      				"seats": "5",
      				"tier": "Team"
      			},
      			"billing_period": 30,
      			"cancel_discount_intervals": null,
      			"cancel_discount_percentage": null,
      			"checkout_styling": null,
      			"created_at": "2023-12-01T05:00:00.401Z",
      			"currency": "usd",
      			"custom_fields": [
      				{
      					"id": "cusf_xxxxxxxxxxxx",
      					"field_type": "text",
      					"name": "Discord username",
      					"order": 0,
      					"placeholder": "e.g. pickaxe_user",
      					"required": true
      				}
      			],
      			"description": "Team tier for up to 5 seats, billed monthly.",
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
      			"internal_notes": "Standard monthly plan",
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
      			"plan_type": "renewal",
      			"product": {
      				"id": "prod_xxxxxxxxxxxxx",
      				"title": "Pickaxe Analytics"
      			},
      			"purchase_url": "https://whop.com/pickaxe-analytics/checkout/plan_zzzzzzzzzzzzz",
      			"release_method": "buy_now",
      			"renewal_price": 79,
      			"sku": "PICKAXE-TEAM-5-MONTHLY",
      			"split_pay_required_payments": null,
      			"stock": null,
      			"strike_through_initial_price": null,
      			"strike_through_renewal_price": null,
      			"three_ds_level": "frictionless_if_required",
      			"title": "Team / 5 seats",
      			"trial_period_days": 7,
      			"unlimited_stock": true,
      			"updated_at": "2023-12-01T05:00:00.401Z",
      			"visibility": "visible"
      		},
      		{
      			"id": "plan_wwwwwwwwwwwww",
      			"account": {
      				"id": "biz_xxxxxxxxxxxxxx",
      				"title": "Pickaxe"
      			},
      			"adaptive_pricing_enabled": true,
      			"attributes": {
      				"seats": "10",
      				"tier": "Team"
      			},
      			"billing_period": 30,
      			"cancel_discount_intervals": null,
      			"cancel_discount_percentage": null,
      			"checkout_styling": null,
      			"created_at": "2023-12-01T05:00:00.401Z",
      			"currency": "usd",
      			"custom_fields": [
      				{
      					"id": "cusf_xxxxxxxxxxxx",
      					"field_type": "text",
      					"name": "Discord username",
      					"order": 0,
      					"placeholder": "e.g. pickaxe_user",
      					"required": true
      				}
      			],
      			"description": "Team tier for up to 10 seats, billed monthly.",
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
      			"internal_notes": "Standard monthly plan",
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
      			"plan_type": "renewal",
      			"product": {
      				"id": "prod_xxxxxxxxxxxxx",
      				"title": "Pickaxe Analytics"
      			},
      			"purchase_url": "https://whop.com/pickaxe-analytics/checkout/plan_wwwwwwwwwwwww",
      			"release_method": "buy_now",
      			"renewal_price": 129,
      			"sku": "PICKAXE-TEAM-10-MONTHLY",
      			"split_pay_required_payments": null,
      			"stock": null,
      			"strike_through_initial_price": null,
      			"strike_through_renewal_price": null,
      			"three_ds_level": "frictionless_if_required",
      			"title": "Team / 10 seats",
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
    </div>
  </Column>
</Columns>
