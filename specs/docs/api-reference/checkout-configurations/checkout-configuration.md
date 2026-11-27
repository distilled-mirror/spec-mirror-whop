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

# Checkout Configurations API

> A Checkout Configuration is a reusable checkout link owned by an account.

In `payment` mode it sells a specific variant; in `setup` mode it collects and saves payment details without charging. Each configuration can also override which payment methods are accepted and how 3D Secure is enforced for that checkout.

Use the Checkout Configurations API to create checkout links for an existing or inline variant, list configurations for an account, retrieve the configuration behind a checkout URL, and delete links that should no longer be used.

> [!NOTE]
>
> Replaces the Legacy [Checkout
> configurations](https://docs.whop.com/api-reference/checkout-configurations/checkout-configuration)
> resource. Existing Legacy integrations keep working; see [API
> versions](https://docs.whop.com/developer/api/versioning.md) for the stability contract.

## Endpoints

| Endpoint | Request |
| - | - |
| [List checkout configurations](https://docs.whop.com/api-reference/checkout-configurations/list-checkout-configurations.md) | GET `/checkout_configurations` |
| [Retrieve a checkout configuration](https://docs.whop.com/api-reference/checkout-configurations/retrieve-a-checkout-configuration.md) | GET `/checkout_configurations/{id}` |
| [Create a checkout configuration](https://docs.whop.com/api-reference/checkout-configurations/create-a-checkout-configuration.md) | POST `/checkout_configurations` |
| [Delete a checkout configuration](https://docs.whop.com/api-reference/checkout-configurations/delete-a-checkout-configuration.md) | DELETE `/checkout_configurations/{id}` |

## Attributes

**id** `string` *required*

Checkout configuration ID, prefixed `ch_`.

**account\_id** `string` *required*

Account ID, prefixed `biz_`.

**affiliate\_code** `string | null`

Affiliate code applied at checkout, or `null` when none is set.

**created\_at** `string` *required*

When the checkout configuration was created, as an ISO 8601 timestamp.

**currency** `string | null`

Currency used for setup-mode payment method availability; defaults to `usd`
when omitted.

**effective\_payment\_method\_configuration** `object | null`

The configuration governing a checkout mounted from this configuration, resolved through every layer (its own overrides, the variant's, and the account's) — the shape a session's `payment_method_configuration` carries. Apply it over the payment method types catalogue for the offerable set. `null` means platform defaults; `payment_method_configuration` stays this configuration's own editable override.

> [accordion] Properties
>
> **disabled** `string[]`
>
> Payment methods explicitly disabled.
>
> **enabled** `string[]`
>
> Payment methods explicitly enabled.
>
> **include\_platform\_defaults** `boolean`
>
> Whether platform default payment methods are included.

**metadata** `object | null`

Custom key-value metadata copied to payments and memberships. `null` without
the `checkout_configuration:basic:read` scope.

**mode** `string` *required*

Controls whether checkout charges the buyer immediately or saves payment details for later.

Available options: `payment`, `setup`

**payment\_method\_configuration** `object | null`

Payment method overrides for this checkout. `null` when it uses the variant or platform defaults.

> [accordion] Properties
>
> **disabled** `string[]`
>
> Payment methods explicitly disabled for checkout.
>
> **enabled** `string[]`
>
> Payment methods explicitly enabled for checkout.
>
> **include\_platform\_defaults** `boolean`
>
> Whether platform default payment methods are included.

**purchase\_url** `string | null`

Checkout URL you can send to customers.

**redirect\_url** `string | null`

URL customers are sent to after checkout, or `null` when no redirect is
configured.

**three\_ds\_level** `string | null`

3D Secure behavior for supported on-session card payments. `mandate_challenge` requires a 3DS challenge before payment processing; `mandate_if_required` mandates a challenge only when the payment processor requires it; `frictionless_if_required` uses the regular frictionless 3DS flow. Payments of \$1,000 or more use `mandate_if_required` unless `mandate_challenge` is selected. Risk and authentication recovery requirements can override the preference. Applies in setup mode; `null` uses frictionless 3DS. Payment mode uses the variant policy.

Available options: `mandate_challenge`, `mandate_if_required`, `frictionless_if_required`

**updated\_at** `string` *required*

When the checkout configuration was last updated, as an ISO 8601 timestamp.

**variant** `object | null`

Variant used for payment checkout. `null` in setup mode.

> [accordion] Properties
>
> **id** `string` *required*
>
> Variant ID, prefixed `plan_`.
>
> **adaptive\_pricing\_enabled** `boolean` *required*
>
> Whether this variant accepts local currency payments via adaptive pricing.
>
> **billing\_period** `integer | null` *required*
>
> Recurring billing interval in days, such as 30 for monthly or 365 for annual.
> `null` for one-time variants.
>
> **billing\_type** `string` *required*
>
> Billing model for the variant.
>
> Available options: `renewal`, `one_time`
>
> **currency** `string` *required*
>
> Three-letter ISO currency code for the variant's prices.
>
> **expiration\_days** `integer | null` *required*
>
> Access duration in days for expiration-based variants.
>
> **initial\_price** `number` *required*
>
> Initial purchase price in the variant currency.
>
> **release\_method** `string` *required*
>
> Sales method for the variant.
>
> Available options: `buy_now`, `waitlist`
>
> **renewal\_price** `number` *required*
>
> Recurring price charged each billing period.
>
> **three\_ds\_level** `string | null` *required*
>
> 3D Secure behavior for supported on-session card payments. `mandate_challenge` requires a 3DS challenge before payment processing; `mandate_if_required` mandates a challenge only when the payment processor requires it; `frictionless_if_required` uses the regular frictionless 3DS flow. Payments of \$1,000 or more use `mandate_if_required` unless `mandate_challenge` is selected. Risk and authentication recovery requirements can override the preference. `null` inherits the account default.
>
> Available options: `mandate_challenge`, `mandate_if_required`, `frictionless_if_required`
>
> **trial\_period\_days** `integer | null` *required*
>
> Free trial days before the first renewal charge.
>
> **visibility** `string` *required*
>
> Whether the variant is visible to customers or hidden from public view.
>
> Available options: `visible`, `hidden`, `archived`, `quick_link`

```json CheckoutConfiguration theme={null}
{
	"id": "ch_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"affiliate_code": "shine-time-partner",
	"created_at": "2026-07-01T12:00:00Z",
	"currency": "usd",
	"metadata": {
		"campaign": "summer-launch"
	},
	"mode": "payment",
	"payment_method_configuration": {
		"enabled": ["card", "apple_pay"],
		"disabled": ["crypto"],
		"include_platform_defaults": true
	},
	"effective_payment_method_configuration": {
		"enabled": ["card", "apple_pay"],
		"disabled": ["crypto"],
		"include_platform_defaults": true
	},
	"variant": {
		"id": "plan_xxxxxxxxxxxxx",
		"visibility": "visible",
		"billing_type": "renewal",
		"release_method": "buy_now",
		"currency": "usd",
		"billing_period": 30,
		"expiration_days": null,
		"initial_price": 29,
		"renewal_price": 29,
		"trial_period_days": 7,
		"three_ds_level": "frictionless_if_required",
		"adaptive_pricing_enabled": true
	},
	"purchase_url": "https://whop.com/checkout/plan_xxxxxxxxxxxxx/?session=ch_xxxxxxxxxxxxxx",
	"redirect_url": "https://shinetime.example/welcome",
	"three_ds_level": "frictionless_if_required",
	"updated_at": "2026-07-02T12:00:00Z"
}
```


## Related topics

- [Checkout Configuration](https://docs.whop.com/api-reference/legacy/checkout-configurations/checkout-configuration.md)
- [Create checkout configuration](https://docs.whop.com/api-reference/legacy/checkout-configurations/create-checkout-configuration.md)
- [Retrieve checkout configuration](https://docs.whop.com/api-reference/legacy/checkout-configurations/retrieve-checkout-configuration.md)
