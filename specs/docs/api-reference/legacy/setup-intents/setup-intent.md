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

# Setup Intent

> A setup intent allows a user to save a payment method for future use without making an immediate purchase.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/setup-intents/setup-intent.md). This page stays fully
> supported.

> [!NOTE]
>
> Create a setup intent with [Create setup intent](https://docs.whop.com/api-reference/legacy/setup-intents/create-setup-intent.md), passing a confirmation token from the [payment elements](https://docs.whop.com/elements/latest/payments/overview) mounted in `mode: "setup"`, or an existing payment method to re-verify. A setup-mode [Checkout Configuration](https://docs.whop.com/api-reference/checkout-configurations/create-a-checkout-configuration.md) creates one for you when the buyer completes that checkout. Either way, listen for the `setup_intent.succeeded` webhook to receive the payment method ID once it's saved.
>
> See [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md) for the full flow.

```typescript theme={null}
// Create a setup intent from a confirmation token the payment elements minted in setup mode
const setupIntent = await client.setupIntents.create({
	account_id: "biz_xxxxxxxxxxxxx",
	confirmation_token: "ctok_xxxxxxxxxxxxxx",
	return_url: "https://yoursite.com/billing/saved",
	metadata: { customer_id: "your_internal_id" },
});

// Hand setupIntent.client_secret to the elements' handleNextAction for any pending step,
// then listen for setup_intent.succeeded on your webhook handler.
```

```json Example theme={null}
{
	"checkout_configuration": {
		"id": "ch_xxxxxxxxxxxxxxx"
	},
	"company": {
		"id": "biz_xxxxxxxxxxxxxx"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"error_message": "Your card was declined.",
	"id": "sint_xxxxxxxxxxxxx",
	"member": {
		"id": "<string>",
		"user": {
			"email": "<string>",
			"id": "<string>",
			"name": "<string>",
			"username": "<string>"
		}
	},
	"metadata": {},
	"payment_method": {
		"card": {
			"brand": "mastercard",
			"exp_month": 42,
			"exp_year": 42,
			"last4": "4242"
		},
		"created_at": "2023-12-01T05:00:00.401Z",
		"id": "payt_xxxxxxxxxxxxx",
		"mailing_address": {
			"city": "<string>",
			"country": "<string>",
			"line1": "<string>",
			"line2": "<string>",
			"name": "<string>",
			"postal_code": "<string>",
			"state": "<string>"
		},
		"payment_method_type": "acss_debit"
	},
	"status": "processing",
	"three_ds_verified": true
}
```

**checkout\_configuration** `object | null` *required*

The checkout session configuration associated with this setup intent. Null if no checkout session was used.

**child attributes**

**id** `string` *required*

The unique identifier for the checkout session.

Example: `ch_xxxxxxxxxxxxxxx`

**company** `object | null` *required*

The company that initiated this setup intent. Null if the company has been deleted.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**created\_at** `string\<date-time\>` *required*

The datetime the setup intent was created.

Example: `2023-12-01T05:00:00.401Z`

**error\_message** `string | null` *required*

A human-readable error message explaining why the setup intent failed. Null if no error occurred.

Example: `Your card was declined.`

**id** `string` *required*

The unique identifier for the setup intent.

Example: `sint_xxxxxxxxxxxxx`

**member** `object | null` *required*

The company member associated with this setup intent. Null if the user is not a member.

**child attributes**

**id** `string` *required*

The unique identifier for the company member.

**user** `object | null` *required*

The user for this member, if any.

**child attributes**

**email** `string | null` *required*

The digital mailing address of the user.

**id** `string` *required*

The unique identifier for the company member user.

**name** `string | null` *required*

The user's full name.

**username** `string` *required*

The whop username.

**metadata** `object | null` *required*

Custom key-value pairs attached to this setup intent. Null if no metadata was
provided.

**payment\_method** `object | null` *required*

The saved payment method created by this setup intent. Null if the setup has not completed successfully.

**child attributes**

**card** `object | null` *required*

The card data associated with the payment method, if its a debit or credit card.

**child attributes**

**brand** `CardBrands | null` *required*

The card network (e.g., visa, mastercard, amex). Null if the brand could not be determined.

Available options: `mastercard`, `visa`, `amex`, `discover`, `unionpay`, `jcb`, `diners`, `link`, `troy`, `visadankort`, `visabancontact`, `china_union_pay`, `rupay`, `jcbrupay`, `elo`, `maestro`, `tarjeta_naranja`, `cirrus`, `nspk_mir`, `verve`, `ebt`, `private_label`, `local_brand`, `uatp`, `wexcard`, `uzcard`, `meeza`, `hrg_store_card`, `girocard`, `fuel_card`, `dankort`, `carnet`, `atm_card`, `china_union_payuzcard`, `codensa`, `cabal`, `hipercard`, `jcblankapay`, `cmi`, `aura`, `accel`, `culiance`, `nyce`, `pulse`, `star`, `eftpos`, `alelo`, `edenred`, `sodexo`, `unknown`

**exp\_month** `integer | null` *required*

The two-digit expiration month of the card (1-12). Null if not available.

Example: `42`

**exp\_year** `integer | null` *required*

The two-digit expiration year of the card (e.g., 27 for 2027). Null if not available.

Example: `42`

**last4** `string | null` *required*

The last four digits of the card number. Null if not available.

Example: `4242`

**created\_at** `string\<date-time\>` *required*

The datetime the payment token was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the payment token.

Example: `payt_xxxxxxxxxxxxx`

**mailing\_address** `object | null` *required*

The mailing address associated with the payment method's user

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

**postal\_code** `string | null` *required*

The postal code of the address.

**state** `string | null` *required*

The state of the address.

**payment\_method\_type** `PaymentMethodTypes` *required*

The payment method type of the payment method

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**status** `SetupIntentStatuses` *required*

The current status of the setup intent.

Available options: `processing`, `succeeded`, `canceled`, `requires_action`

**three\_ds\_verified** `boolean` *required*

Whether 3D Secure authentication was completed when this payment method was
set up.


## Related topics

- [Whop CLI Commands](https://docs.whop.com/cli/commands.md)
- [Setup Intents API](https://docs.whop.com/api-reference/setup-intents/setup-intent.md)
- [List setup intents](https://docs.whop.com/api-reference/legacy/setup-intents/list-setup-intents.md)
