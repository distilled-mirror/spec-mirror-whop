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

# Refund

> A refund represents a full or partial reversal of a payment, including the amount, status, and payment provider.

```json Example theme={null}
{
	"amount": 6.9,
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "usd",
	"id": "rf_xxxxxxxxxxxxxxx",
	"metadata": {},
	"payment": {
		"billing_reason": "subscription_create",
		"card_brand": "mastercard",
		"card_last4": "4242",
		"created_at": "2023-12-01T05:00:00.401Z",
		"currency": "usd",
		"dispute_alerted_at": "2023-12-01T05:00:00.401Z",
		"id": "pay_xxxxxxxxxxxxxx",
		"member": {
			"id": "<string>",
			"phone": "<string>"
		},
		"membership": {
			"id": "mem_xxxxxxxxxxxxxx",
			"status": "trialing"
		},
		"metadata": {},
		"paid_at": "2023-12-01T05:00:00.401Z",
		"payment_method_type": "acss_debit",
		"plan": {
			"id": "plan_xxxxxxxxxxxxx",
			"metadata": {}
		},
		"product": {
			"id": "prod_xxxxxxxxxxxxx",
			"metadata": {}
		},
		"subtotal": 6.9,
		"tax_amount": 6.9,
		"tax_behavior": "exclusive",
		"tax_refunded_amount": 6.9,
		"total": 6.9,
		"usd_total": 6.9,
		"user": {
			"email": "john.doe@example.com",
			"id": "user_xxxxxxxxxxxxx",
			"name": "Marcus Webb",
			"username": "marcuswebb"
		}
	},
	"provider": "stripe",
	"provider_created_at": "2023-12-01T05:00:00.401Z",
	"reference_status": "available",
	"reference_type": "acquirer_reference_number",
	"reference_value": "74850120752",
	"status": "pending"
}
```

**amount** `number` *required*

The refunded amount as a decimal in the specified currency, such as 10.43 for \$10.43 USD.

Example: `6.9`

**created\_at** `string\<date-time\>` *required*

The datetime the refund was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies` *required*

The three-letter ISO currency code for the refunded amount.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**id** `string` *required*

The unique identifier for the refund.

Example: `rf_xxxxxxxxxxxxxxx`

**metadata** `object | null` *required*

The custom metadata attached when this refund was requested.

**payment** `object | null` *required*

The original payment that this refund was issued against. Null if the payment is no longer available.

**child attributes**

**billing\_reason** `BillingReasons | null` *required*

The machine-readable reason this charge was created, such as initial subscription purchase, renewal cycle, or one-time payment.

Available options: `subscription_create`, `subscription_cycle`, `subscription_update`, `one_time`, `manual`, `subscription`

**card\_brand** `CardBrands | null` *required*

Card network reported by the processor (e.g., 'visa', 'mastercard', 'amex'). Present only when the payment method type is 'card'.

Available options: `mastercard`, `visa`, `amex`, `discover`, `unionpay`, `jcb`, `diners`, `link`, `troy`, `visadankort`, `visabancontact`, `china_union_pay`, `rupay`, `jcbrupay`, `elo`, `maestro`, `tarjeta_naranja`, `cirrus`, `nspk_mir`, `verve`, `ebt`, `private_label`, `local_brand`, `uatp`, `wexcard`, `uzcard`, `meeza`, `hrg_store_card`, `girocard`, `fuel_card`, `dankort`, `carnet`, `atm_card`, `china_union_payuzcard`, `codensa`, `cabal`, `hipercard`, `jcblankapay`, `cmi`, `aura`, `accel`, `culiance`, `nyce`, `pulse`, `star`, `eftpos`, `alelo`, `edenred`, `sodexo`, `unknown`

**card\_last4** `string | null` *required*

The last four digits of the card used to make this payment. Null if the payment was not made with a card.

Example: `4242`

**created\_at** `string\<date-time\>` *required*

The datetime the payment was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies` *required*

The three-letter ISO currency code for this payment (e.g., 'usd', 'eur').

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**dispute\_alerted\_at** `string\<date-time\> | null` *required*

When an alert came in that this transaction will be disputed

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the payment.

Example: `pay_xxxxxxxxxxxxxx`

**member** `object | null` *required*

The member attached to this payment.

**child attributes**

**id** `string` *required*

The unique identifier for the company member.

**phone** `string | null` *required*

The phone number for the member, if available.

**membership** `object | null` *required*

The membership attached to this payment.

**child attributes**

**id** `string` *required*

The unique identifier for the membership.

Example: `mem_xxxxxxxxxxxxxx`

**status** `MembershipStatus` *required*

The state of the membership.

Available options: `trialing`, `active`, `past_due`, `completed`, `canceled`, `expired`, `unresolved`, `scheduled`, `drafted`, `canceling`

**metadata** `object | null` *required*

The custom metadata stored on this payment. This will be copied over to the
checkout configuration for which this payment was made

**paid\_at** `string\<date-time\> | null` *required*

The time at which this payment was successfully collected. Null if the payment has not yet succeeded. As a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**payment\_method\_type** `PaymentMethodTypes | null` *required*

The type of payment instrument used for this payment (e.g., card, Cash App, iDEAL, Klarna, crypto). Null when the processor does not supply a type.

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**plan** `object | null` *required*

The plan attached to this payment.

**child attributes**

**id** `string` *required*

The unique identifier for the plan.

Example: `plan_xxxxxxxxxxxxx`

**metadata** `object | null` *required*

Custom key-value pairs stored on the plan. Included in webhook payloads for payment and membership events. Max 50 keys, 100 chars per key, 500 chars per string value. The reserved keys `custom_cta` and `custom_cta_url`, when set, override the product's checkout call to action for this plan.

**product** `object | null` *required*

The product this payment was made for

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**metadata** `object | null` *required*

Custom key-value pairs stored on the product and included in payment and membership webhook payloads. Max 50 keys, 100 characters per key, 500 characters per string value.

**subtotal** `number | null` *required*

The subtotal to show to the creator (excluding buyer fees).

Example: `6.9`

**tax\_amount** `number | null` *required*

The calculated amount of the sales/VAT tax (if applicable).

Example: `6.9`

**tax\_behavior** `ReceiptTaxBehaviors | null` *required*

The type of tax inclusivity applied to the payment, for determining whether the tax is included in the final price, or paid on top.

Available options: `exclusive`, `inclusive`, `unspecified`, `unable_to_collect`

**tax\_refunded\_amount** `number | null` *required*

The amount of tax that has been refunded (if applicable).

Example: `6.9`

**total** `number | null` *required*

The total to show to the creator (excluding buyer fees).

Example: `6.9`

**usd\_total** `number | null` *required*

The total in USD to show to the creator (excluding buyer fees).

Example: `6.9`

**user** `object | null` *required*

The user that made this payment.

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

**provider** `PaymentProviders` *required*

The payment provider that processed the refund.

Available options: `stripe`, `coinbase`, `paypal`, `apple`, `sezzle`, `splitit`, `platform_balance`, `multi_psp`, `adyen`, `claritypay`, `flex_pay`, `checkout_dot_com`, `airwallex`, `coinflow`, `sequra`, `dlocal`, `masspay`, `braintree`

**provider\_created\_at** `string\<date-time\> | null` *required*

The timestamp when the refund was created in the payment provider's system. Null if not available from the provider.

Example: `2023-12-01T05:00:00.401Z`

**reference\_status** `RefundReferenceStatuses | null` *required*

The availability status of the refund tracking reference from the payment processor. Null if no reference was provided.

Available options: `available`, `pending`, `unavailable`

**reference\_type** `RefundReferenceTypes | null` *required*

The type of tracking reference provided by the payment processor, such as an acquirer reference number. Null if no reference was provided.

Available options: `acquirer_reference_number`, `retrieval_reference_number`, `system_trace_audit_number`

**reference\_value** `string | null` *required*

The tracking reference value from the payment processor, used to trace the refund through banking networks. Null if no reference was provided.

Example: `74850120752`

**status** `RefundStatuses` *required*

The current processing status of the refund, such as pending, succeeded, or failed.

Available options: `pending`, `requires_action`, `succeeded`, `failed`, `canceled`


## Related topics

- [Refunds API](https://docs.whop.com/api-reference/refunds/refund.md)
- [Refunds and Disputes](https://docs.whop.com/developer/guides/refunds-and-disputes.md)
- [Retrieve Refund](https://docs.whop.com/api-reference/refunds/retrieve-refund.md)
