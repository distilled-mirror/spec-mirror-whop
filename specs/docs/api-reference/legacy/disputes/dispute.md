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

# Dispute

> A dispute is a chargeback or payment challenge filed against a company, including evidence and response status.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/disputes/list-disputes.md), the versioned API that new
> integrations should build on. This page stays fully supported.

```json Example theme={null}
{
	"access_activity_log": "192.168.1.1 - 2024-01-15 12:00:00 UTC",
	"amount": 6.9,
	"billing_address": "123 Main St, New York, NY 10001",
	"cancellation_policy_attachment": {
		"content_type": "image/jpeg",
		"filename": "document.pdf",
		"id": "<string>",
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"cancellation_policy_disclosure": "All sales are final. No refunds after 30 days.",
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "<string>"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "usd",
	"customer_communication_attachment": {
		"content_type": "image/jpeg",
		"filename": "document.pdf",
		"id": "<string>",
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"customer_email_address": "customer@example.com",
	"customer_name": "Dana Whitfield",
	"editable": true,
	"id": "dspt_xxxxxxxxxxxxx",
	"needs_response_by": "2023-12-01T05:00:00.401Z",
	"notes": "The ceramic coating was completed and the vehicle collected before the dispute.",
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
		"paid_at": "2023-12-01T05:00:00.401Z",
		"payment_instrument": {
			"display_name": "<string>",
			"icons": {
				"square": {
					"dark": {
						"svg": "<string>"
					},
					"light": {
						"svg": "<string>"
					}
				}
			},
			"installment_count": 42,
			"payment_method_type": "<string>"
		},
		"payment_method_type": "acss_debit",
		"subtotal": 6.9,
		"total": 6.9,
		"usd_total": 6.9,
		"user": {
			"email": "john.doe@example.com",
			"id": "user_xxxxxxxxxxxxx",
			"name": "Marcus Webb",
			"username": "marcuswebb"
		}
	},
	"plan": {
		"id": "plan_xxxxxxxxxxxxx"
	},
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	},
	"product_description": "Monthly membership for maintenance washes and interior vacuuming.",
	"reason": "Product Not Received",
	"reason_code": "10.4",
	"refund_policy_attachment": {
		"content_type": "image/jpeg",
		"filename": "document.pdf",
		"id": "<string>",
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"refund_policy_disclosure": "Refunds available within 14 days of purchase.",
	"refund_refusal_explanation": "The customer exceeded the refund window by 60 days.",
	"service_date": "2024-01-15",
	"status": "warning_needs_response",
	"uncategorized_attachment": {
		"content_type": "image/jpeg",
		"filename": "document.pdf",
		"id": "<string>",
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"visa_rdr": true
}
```

**access\_activity\_log** `string | null` *required*

A log of IP-based access activity for the customer on Whop, submitted as evidence in the dispute.

Example: `192.168.1.1 - 2024-01-15 12:00:00 UTC`

**amount** `number` *required*

The disputed amount in the specified currency, formatted as a decimal.

Example: `6.9`

**billing\_address** `string | null` *required*

The customer's billing address from their payment details, submitted as evidence in the dispute.

Example: `123 Main St, New York, NY 10001`

**cancellation\_policy\_attachment** `object | null` *required*

The cancellation policy document uploaded as dispute evidence. Null if no cancellation policy has been provided.

**child attributes**

**content\_type** `string | null` *required*

Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.

Example: `image/jpeg`

**filename** `string | null` *required*

The original filename of the uploaded attachment, including its file extension.

Example: `document.pdf`

**id** `string` *required*

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**cancellation\_policy\_disclosure** `string | null` *required*

A text disclosure describing the company's cancellation policy, submitted as dispute evidence.

Example: `All sales are final. No refunds after 30 days.`

**company** `object | null` *required*

The company that the dispute was filed against.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**title** `string` *required*

The written name of the company.

**created\_at** `string\<date-time\> | null` *required*

The datetime the dispute was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies` *required*

The three-letter ISO currency code for the disputed amount.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**customer\_communication\_attachment** `object | null` *required*

Evidence of customer communication or product usage, uploaded as a dispute attachment. Null if not provided.

**child attributes**

**content\_type** `string | null` *required*

Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.

Example: `image/jpeg`

**filename** `string | null` *required*

The original filename of the uploaded attachment, including its file extension.

Example: `document.pdf`

**id** `string` *required*

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**customer\_email\_address** `string | null` *required*

The customer's email address from their payment details, included in the evidence packet sent to the payment processor. Editable before submission.

Example: `customer@example.com`

**customer\_name** `string | null` *required*

The customer's full name from their payment details, included in the evidence packet sent to the payment processor. Editable before submission.

Example: `Dana Whitfield`

**editable** `boolean | null` *required*

Whether the dispute evidence can still be edited and submitted.

**id** `string` *required*

The unique identifier for the dispute.

Example: `dspt_xxxxxxxxxxxxx`

**needs\_response\_by** `string\<date-time\> | null` *required*

The deadline by which dispute evidence must be submitted. Null if no response deadline is set.

Example: `2023-12-01T05:00:00.401Z`

**notes** `string | null` *required*

Additional freeform notes submitted by the company as part of the dispute evidence.

Example: `The ceramic coating was completed and the vehicle collect…`

**payment** `object | null` *required*

The original payment that was disputed.

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

**paid\_at** `string\<date-time\> | null` *required*

The time at which this payment was successfully collected. Null if the payment has not yet succeeded. As a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**payment\_instrument** `object | null` *required*

The instrument this payment was made with, shaped for display: the method type, a buyer-facing name, the standard icon set, and the card facts when it was a card. Null when the receipt names no payment method.

**child attributes**

**display\_name** `string` *required*

Buyer-facing instrument name — "Visa •••• 4242" when the card surfaced, else
the method's own name ("Klarna").

**icons** `object` *required*

The standard icon set: square and card shapes, each in light and dark colorways.

**child attributes**

**square** `object` *required*

The square tile (32x32).

**child attributes**

**dark** `object` *required*

The colorway for dark surfaces.

**child attributes**

**svg** `string` *required*

The vector file. Prefer this everywhere SVG renders.

**light** `object` *required*

The colorway for light surfaces.

**child attributes**

**svg** `string` *required*

The vector file. Prefer this everywhere SVG renders.

**installment\_count** `integer | null` *required*

Installment methods only: how many payments the charge splits into. Data, not copy — compose and translate the label client-side.

Example: `42`

**payment\_method\_type** `string` *required*

The payment method type identifier, e.g. `card`, `klarna`, `apple_pay`.

**payment\_method\_type** `PaymentMethodTypes | null` *required*

The type of payment instrument used for this payment (e.g., card, Cash App, iDEAL, Klarna, crypto). Null when the processor does not supply a type.

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**subtotal** `number | null` *required*

The subtotal to show to the creator (excluding buyer fees).

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

**plan** `object | null` *required*

The plan associated with the disputed payment. Null if the dispute is not linked to a specific plan.

**child attributes**

**id** `string` *required*

The unique identifier for the plan.

Example: `plan_xxxxxxxxxxxxx`

**product** `object | null` *required*

The product associated with the disputed payment. Null if the dispute is not linked to a specific product.

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`

**product\_description** `string | null` *required*

A description of the product or service provided, submitted as dispute evidence.

Example: `Monthly membership for maintenance washes and interior va…`

**reason** `string | null` *required*

A human-readable reason for the dispute.

Example: `Product Not Received`

**reason\_code** `string | null` *required*

The card network reason code for the dispute. Null when the payment processor did not provide one.

Example: `10.4`

**refund\_policy\_attachment** `object | null` *required*

The refund policy document uploaded as dispute evidence. Null if no refund policy has been provided.

**child attributes**

**content\_type** `string | null` *required*

Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.

Example: `image/jpeg`

**filename** `string | null` *required*

The original filename of the uploaded attachment, including its file extension.

Example: `document.pdf`

**id** `string` *required*

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**refund\_policy\_disclosure** `string | null` *required*

A text disclosure describing the company's refund policy, submitted as dispute evidence.

Example: `Refunds available within 14 days of purchase.`

**refund\_refusal\_explanation** `string | null` *required*

An explanation from the company for why a refund was refused, submitted as dispute evidence.

Example: `The customer exceeded the refund window by 60 days.`

**service\_date** `string | null` *required*

The date when the product or service was delivered to the customer, submitted as dispute evidence.

Example: `2024-01-15`

**status** `DisputeStatuses` *required*

The current status of the dispute lifecycle, such as needs\_response, under\_review, won, or lost.

Available options: `warning_needs_response`, `warning_under_review`, `warning_closed`, `needs_response`, `under_review`, `won`, `lost`, `closed`, `other`

**uncategorized\_attachment** `object | null` *required*

An additional attachment that does not fit into the standard evidence categories. Null if not provided.

**child attributes**

**content\_type** `string | null` *required*

Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.

Example: `image/jpeg`

**filename** `string | null` *required*

The original filename of the uploaded attachment, including its file extension.

Example: `document.pdf`

**id** `string` *required*

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**visa\_rdr** `boolean` *required*

Whether the dispute was automatically resolved through Visa Rapid Dispute
Resolution (RDR).


## Related topics

- [Disputes API](https://docs.whop.com/api-reference/disputes/dispute.md)
- [Refunds and Disputes](https://docs.whop.com/developer/guides/refunds-and-disputes.md)
- [Accept Dispute](https://docs.whop.com/api-reference/disputes/accept-dispute.md)
