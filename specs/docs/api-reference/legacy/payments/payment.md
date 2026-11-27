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

# Payment

> A payment represents a completed or attempted charge. Payments track the amount, status, currency, and payment method used.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/payments/retrieve-payment-status.md). This page stays fully
> supported.

```json Example theme={null}
{
	"amount_after_fees": 6.9,
	"application_fee": {
		"amount": 6.9,
		"amount_captured": 6.9,
		"amount_refunded": 6.9,
		"created_at": "2023-12-01T05:00:00.401Z",
		"currency": "usd",
		"id": "apfee_xxxxxxxxxxxx"
	},
	"auto_refunded": true,
	"billing_address": {
		"city": "<string>",
		"country": "<string>",
		"line1": "<string>",
		"line2": "<string>",
		"name": "<string>",
		"postal_code": "<string>",
		"state": "<string>"
	},
	"billing_reason": "subscription_create",
	"card_brand": "mastercard",
	"card_exp_month": 6,
	"card_exp_year": 2027,
	"card_last4": "4242",
	"checkout_configuration_id": "ch_xxxxxxxxxxxxxxx",
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"route": "<string>",
		"title": "<string>"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "usd",
	"customer_phone": "<string>",
	"decline_code": "insufficient_funds",
	"dispute_alerted_at": "2023-12-01T05:00:00.401Z",
	"disputes": [
		{
			"amount": 6.9,
			"currency": "usd",
			"editable": true,
			"id": "dspt_xxxxxxxxxxxxx",
			"needs_response_by": "2023-12-01T05:00:00.401Z",
			"notes": "The ceramic coating was completed and the vehicle collected before the dispute.",
			"reason": "Product Not Received",
			"status": "warning_needs_response"
		}
	],
	"failure_message": "<string>",
	"fees": [
		{
			"amount": 6.9,
			"currency": "usd",
			"name": "<string>",
			"type": "payment_processing_percentage_fee"
		}
	],
	"financing_installments_count": 42,
	"financing_transactions": [
		{
			"amount": 6.9,
			"created_at": "2023-12-01T05:00:00.401Z",
			"id": "ptx_xxxxxxxxxxxxxx",
			"status": "succeeded",
			"transaction_type": "purchase"
		}
	],
	"id": "pay_xxxxxxxxxxxxxx",
	"last_payment_attempt": "2023-12-01T05:00:00.401Z",
	"member": {
		"id": "<string>",
		"phone": "<string>"
	},
	"membership": {
		"id": "mem_xxxxxxxxxxxxxx",
		"phone_number": "<string>",
		"status": "trialing"
	},
	"metadata": {},
	"needs_tracking": true,
	"next_payment_attempt": "2023-12-01T05:00:00.401Z",
	"paid_at": "2023-12-01T05:00:00.401Z",
	"payment_instrument": {
		"card": {
			"brand": "<string>",
			"last4": "<string>"
		},
		"display_name": "<string>",
		"icons": {
			"card": {
				"dark": {
					"png_1x": "<string>",
					"png_2x": "<string>",
					"png_4x": "<string>",
					"svg": "<string>"
				},
				"light": {
					"png_1x": "<string>",
					"png_2x": "<string>",
					"png_4x": "<string>",
					"svg": "<string>"
				}
			},
			"square": {
				"dark": {
					"png_1x": "<string>",
					"png_2x": "<string>",
					"png_4x": "<string>",
					"svg": "<string>"
				},
				"light": {
					"png_1x": "<string>",
					"png_2x": "<string>",
					"png_4x": "<string>",
					"svg": "<string>"
				}
			}
		},
		"installment_count": 42,
		"payment_method_type": "<string>"
	},
	"payment_method": {
		"card": {
			"brand": "mastercard",
			"exp_month": 42,
			"exp_year": 42,
			"fingerprint": "<string>",
			"last4": "4242"
		},
		"created_at": "2023-12-01T05:00:00.401Z",
		"id": "payt_xxxxxxxxxxxxx",
		"payment_method_type": "acss_debit"
	},
	"payment_method_type": "acss_debit",
	"payments_failed": 42,
	"plan": {
		"id": "plan_xxxxxxxxxxxxx",
		"internal_notes": "<string>",
		"metadata": {}
	},
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"metadata": {},
		"route": "ceramic-coating-package",
		"title": "Ceramic Coating Package"
	},
	"promo_code": {
		"amount_off": 6.9,
		"base_currency": "usd",
		"code": "<string>",
		"id": "promo_xxxxxxxxxxxx",
		"number_of_intervals": 42,
		"promo_type": "percentage"
	},
	"recovery_url": "<string>",
	"refundable": true,
	"refunded_amount": 6.9,
	"refunded_at": "2023-12-01T05:00:00.401Z",
	"refunds": [
		{
			"amount": 6.9,
			"created_at": "2023-12-01T05:00:00.401Z",
			"currency": "usd",
			"id": "rf_xxxxxxxxxxxxxxx",
			"status": "pending"
		}
	],
	"resolutions": [
		{
			"customer_appealed": true,
			"customer_response_actions": ["respond"],
			"due_date": "2023-12-01T05:00:00.401Z",
			"id": "reso_xxxxxxxxxxxxx",
			"issue": "forgot_to_cancel",
			"merchant_appealed": true,
			"merchant_response_actions": ["accept"],
			"platform_response_actions": ["request_buyer_info"],
			"status": "merchant_response_needed"
		}
	],
	"retryable": true,
	"risk_score": 42,
	"risk_signals": {},
	"settlement_amount": 6.9,
	"settlement_currency": "usd",
	"settlement_exchange_rate": 6.9,
	"settlement_time_at": "2023-12-01T05:00:00.401Z",
	"shipment": {
		"carrier": "usps",
		"id": "ship_xxxxxxxxxxxxx",
		"status": "unknown",
		"tracking_number": "9400111899223456789012",
		"tracking_url": "https://track.aftership.com/9400111899223456789012"
	},
	"shipping_address": {
		"city": "<string>",
		"country": "<string>",
		"line1": "<string>",
		"line2": "<string>",
		"name": "<string>",
		"postal_code": "<string>",
		"state": "<string>"
	},
	"sku": "<string>",
	"status": "draft",
	"substatus": "succeeded",
	"subtotal": 6.9,
	"tax_amount": 6.9,
	"tax_behavior": "exclusive",
	"tax_refunded_amount": 6.9,
	"three_ds_verified": true,
	"total": 6.9,
	"updated_at": "2023-12-01T05:00:00.401Z",
	"usd_total": 6.9,
	"user": {
		"email": "john.doe@example.com",
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	},
	"verification_checks": {
		"address_line1": "<string>",
		"authorization_code": "<string>",
		"card_holder_name": "<string>",
		"card_security_code": "<string>",
		"zip_code": "<string>"
	},
	"voidable": true
}
```

**amount\_after\_fees** `number` *required*

How much the payment is for after fees

Example: `6.9`

**application\_fee** `object | null` *required*

The application fee charged on this payment.

**child attributes**

**amount** `number` *required*

The application fee amount.

Example: `6.9`

**amount\_captured** `number` *required*

The amount of the application fee that has been captured.

Example: `6.9`

**amount\_refunded** `number` *required*

The amount of the application fee that has been refunded.

Example: `6.9`

**created\_at** `string\<date-time\>` *required*

The datetime the application fee was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies` *required*

The currency of the application fee.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**id** `string` *required*

The unique identifier for the application fee.

Example: `apfee_xxxxxxxxxxxx`

**auto\_refunded** `boolean` *required*

Whether this payment was auto refunded or not

**billing\_address** `object | null` *required*

The address of the user who made the payment.

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

**billing\_reason** `BillingReasons | null` *required*

The machine-readable reason this charge was created, such as initial subscription purchase, renewal cycle, or one-time payment.

Available options: `subscription_create`, `subscription_cycle`, `subscription_update`, `one_time`, `manual`, `subscription`

**card\_brand** `CardBrands | null` *required*

Card network reported by the processor (e.g., 'visa', 'mastercard', 'amex'). Present only when the payment method type is 'card'.

Available options: `mastercard`, `visa`, `amex`, `discover`, `unionpay`, `jcb`, `diners`, `link`, `troy`, `visadankort`, `visabancontact`, `china_union_pay`, `rupay`, `jcbrupay`, `elo`, `maestro`, `tarjeta_naranja`, `cirrus`, `nspk_mir`, `verve`, `ebt`, `private_label`, `local_brand`, `uatp`, `wexcard`, `uzcard`, `meeza`, `hrg_store_card`, `girocard`, `fuel_card`, `dankort`, `carnet`, `atm_card`, `china_union_payuzcard`, `codensa`, `cabal`, `hipercard`, `jcblankapay`, `cmi`, `aura`, `accel`, `culiance`, `nyce`, `pulse`, `star`, `eftpos`, `alelo`, `edenred`, `sodexo`, `unknown`

**card\_exp\_month** `integer | null` *required*

The expiration month (1-12) of the card used for this payment. Falls back to the declined card on failed payments with no saved card. Null when the payment was not made with a card or the expiry is unavailable.

Example: `6`

**card\_exp\_year** `integer | null` *required*

The four-digit expiration year of the card used for this payment. Falls back to the declined card on failed payments with no saved card. Null when the payment was not made with a card or the expiry is unavailable.

Example: `2027`

**card\_last4** `string | null` *required*

The last four digits of the card used to make this payment. Null if the payment was not made with a card.

Example: `4242`

**checkout\_configuration\_id** `string | null` *required*

The ID of the checkout session/configuration that produced this payment, if any. Use this to map payments back to the checkout configuration that created them.

Example: `ch_xxxxxxxxxxxxxxx`

**company** `object | null` *required*

The company for the payment.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**route** `string` *required*

The slug/route of the company on the Whop site.

**title** `string` *required*

The written name of the company.

**created\_at** `string\<date-time\>` *required*

The datetime the payment was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies` *required*

The three-letter ISO currency code for this payment (e.g., 'usd', 'eur').

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**customer\_phone** `string | null` *required*

Phone number the customer provided at checkout, or their verified phone number
when your checkout requires phone verification. `null` when no phone number
was collected.

**decline\_code** `PaymentDeclineCodes | null` *required*

The reason the payment was declined. Null if the payment did not fail.

Available options: `insufficient_funds`, `lost_card`, `stolen_card`, `expired_card`, `suspected_fraud`, `invalid_card_number`, `invalid_cvc`, `invalid_cvc_or_expiration`, `incorrect_pin`, `authentication_required`, `card_not_supported`, `currency_not_supported`, `duplicate_transaction`, `generic_decline`, `invalid_account`, `invalid_amount`, `processing_error`, `restricted_card`, `card_velocity_exceeded`, `contact_issuer`, `card_declined_by_issuer`, `bank_declined`, `regulatory_blocked`, `transaction_not_permitted`, `transaction_stopped`, `card_type_not_supported`, `issuer_not_found`, `closed_account`, `issuer_unavailable`, `invalid_zip`, `invalid_billing_address`, `invalid_expiry_month`, `invalid_expiry_year`, `invalid_expiry`, `invalid_transaction`, `cannot_authorize`, `pin_required`, `pin_try_exceeded`, `provider_declined`, `high_risk`, `test_mode_decline`, `merchant_blacklist`, `reenter_transaction`, `invalid_pin`, `pin_required_as`, `withdrawal_count_limit_exceeded`, `invalid_country`, `issuer_error`, `invalid_card_holder_name`, `no_accounts`, `transaction_cancelled`, `three_d_secure_success`, `three_d_secure_canceled`, `three_d_secure_invalid_card_number`, `three_d_secure_generic_error`, `three_d_secure_timeout`, `three_d_secure_failed`, `three_d_secure_card_not_enrolled`, `three_d_secure_fraud`, `three_d_secure_too_many_attempts`, `three_d_secure_rejected_by_bank`, `three_d_secure_reported_lost_or_stolen`, `blocked_by_cardholder`, `test_mode_test_card`, `try_again_later`, `transaction_not_allowed`, `bank_insufficient_funds`, `bank_account_not_found`, `bank_account_closed`, `bank_account_frozen`, `bank_invalid_routing_number`, `bank_non_transaction_account`, `bank_authorization_revoked`, `bank_payment_stopped`, `bank_not_authorized`, `bank_account_holder_deceased`, `bank_duplicate`, `bank_amount_error`, `bank_regulatory_blocked`, `bank_details_invalid`, `bank_processing_error`, `bank_generic_decline`, `sepa_invalid_iban`, `sepa_no_mandate`, `sepa_mandate_data_invalid`, `sepa_disputed`, `sepa_refused_by_customer`, `sepa_generic_decline`

**dispute\_alerted\_at** `string\<date-time\> | null` *required*

When an alert came in that this transaction will be disputed

Example: `2023-12-01T05:00:00.401Z`

**disputes** `array\<object\> | null` *required*

The disputes attached to this payment. Null if the actor in context does not have the payment:dispute:read permission.

**child attributes**

**amount** `number` *required*

The disputed amount in the specified currency, formatted as a decimal.

Example: `6.9`

**currency** `Currencies` *required*

The three-letter ISO currency code for the disputed amount.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

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

**reason** `string | null` *required*

A human-readable reason for the dispute.

Example: `Product Not Received`

**status** `DisputeStatuses` *required*

The current status of the dispute lifecycle, such as needs\_response, under\_review, won, or lost.

Available options: `warning_needs_response`, `warning_under_review`, `warning_closed`, `needs_response`, `under_review`, `won`, `lost`, `closed`, `other`

**failure\_message** `string | null` *required*

If the payment failed, the reason for the failure.

**fees** `array\<object\>` *required*

The fees associated with this specific payment.

**child attributes**

**amount** `number` *required*

The value or amount to display for the fee.

Example: `6.9`

**currency** `Currencies` *required*

The currency of the fee.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**name** `string` *required*

The label to display for the fee.

**type** `SpecificFeeOrigins` *required*

The specific origin of the fee, if applicable.

Available options: `stripe_domestic_processing_fee`, `stripe_international_processing_fee`, `stripe_fixed_processing_fee`, `stripe_billing_fee`, `stripe_radar_fee`, `sales_tax_remittance`, `sales_tax_remittance_reversal`, `stripe_sales_tax_fee`, `whop_processing_fee`, `marketplace_affiliate_fee`, `affiliate_fee`, `crypto_fee`, `stripe_standard_processing_fee`, `paypal_fee`, `stripe_payout_fee`, `dispute_fee`, `dispute_alert_fee`, `dispute_representment_fee`, `apple_processing_fee`, `buyer_fee`, `sezzle_processing_fee`, `splitit_processing_fee`, `platform_balance_processing_fee`, `payment_processing_percentage_fee`, `payment_processing_fixed_fee`, `cross_border_percentage_fee`, `fx_percentage_fee`, `orchestration_percentage_fee`, `three_ds_fixed_fee`, `billing_percentage_fee`, `revshare_percentage_fee`, `application_fee`, `high_risk_merchant_fee`, `economic_intelligence_percentage_fee`, `payment_return_fee`

Example: `payment_processing_percentage_fee`

**financing\_installments\_count** `integer | null` *required*

The number of financing installments for the payment. Present if the payment is a financing payment (e.g. Splitit, Klarna, etc.).

Example: `42`

**financing\_transactions** `array\<object\>` *required*

The financing transactions attached to this payment. Present if the payment is a financing payment (e.g. Splitit, Klarna, etc.).

**child attributes**

**amount** `number` *required*

The amount of the payment transaction.

Example: `6.9`

**created\_at** `string\<date-time\>` *required*

The date and time the payment transaction was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the payment transaction.

Example: `ptx_xxxxxxxxxxxxxx`

**status** `PaymentTransactionStatuses` *required*

The status of the payment transaction.

Available options: `succeeded`, `declined`, `error`, `pending`, `created`, `expired`, `won`, `rejected`, `lost`, `prevented`, `canceled`

**transaction\_type** `PaymentTransactionTypes` *required*

The type of the payment transaction.

Available options: `purchase`, `authorize`, `capture`, `refund`, `canceled`, `verify`, `chargeback`, `pre_chargeback`, `three_d_secure`, `fraud_screening`, `authorization`, `installment`, `deposit`

**id** `string` *required*

The unique identifier for the payment.

Example: `pay_xxxxxxxxxxxxxx`

**last\_payment\_attempt** `string\<date-time\> | null` *required*

The time of the last payment attempt.

Example: `2023-12-01T05:00:00.401Z`

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

**phone\_number** `string | null` *required*

The phone number associated with this membership.

**status** `MembershipStatus` *required*

The state of the membership.

Available options: `trialing`, `active`, `past_due`, `completed`, `canceled`, `expired`, `unresolved`, `scheduled`, `drafted`, `canceling`

**metadata** `object | null` *required*

The custom metadata stored on this payment. This will be copied over to the
checkout configuration for which this payment was made

**needs\_tracking** `boolean | null` *required*

Whether this payment is holding funds until the order ships and has no
tracking number yet.

**next\_payment\_attempt** `string\<date-time\> | null` *required*

The time of the next schedule payment retry.

Example: `2023-12-01T05:00:00.401Z`

**paid\_at** `string\<date-time\> | null` *required*

The time at which this payment was successfully collected. Null if the payment has not yet succeeded. As a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**payment\_instrument** `object | null` *required*

The instrument this payment was made with, shaped for display: the method type, a buyer-facing name, the standard icon set, and the card facts when it was a card. Null when the receipt names no payment method.

**child attributes**

**card** `object | null` *required*

Card payments only: the card's network and last four.

**child attributes**

**brand** `string` *required*

The network identifier (`visa`, `amex`, …), matching `card.networks` entries
and saved card payment methods.

**last4** `string | null` *required*

The card's last four digits, when captured.

**display\_name** `string` *required*

Buyer-facing instrument name — "Visa •••• 4242" when the card surfaced, else
the method's own name ("Klarna").

**icons** `object` *required*

The standard icon set: square and card shapes, each in light and dark colorways.

**child attributes**

**card** `object` *required*

The credit-card-proportioned tile (48x30).

**child attributes**

**dark** `object` *required*

The colorway for dark surfaces.

**child attributes**

**png\_1x** `string` *required*

Raster fallback at the shape's native size.

**png\_2x** `string` *required*

Raster fallback at double density.

**png\_4x** `string` *required*

Raster fallback at quadruple density.

**svg** `string` *required*

The vector file. Prefer this everywhere SVG renders.

**light** `object` *required*

The colorway for light surfaces.

**child attributes**

**png\_1x** `string` *required*

Raster fallback at the shape's native size.

**png\_2x** `string` *required*

Raster fallback at double density.

**png\_4x** `string` *required*

Raster fallback at quadruple density.

**svg** `string` *required*

The vector file. Prefer this everywhere SVG renders.

**square** `object` *required*

The square tile (32x32).

**child attributes**

**dark** `object` *required*

The colorway for dark surfaces.

**child attributes**

**png\_1x** `string` *required*

Raster fallback at the shape's native size.

**png\_2x** `string` *required*

Raster fallback at double density.

**png\_4x** `string` *required*

Raster fallback at quadruple density.

**svg** `string` *required*

The vector file. Prefer this everywhere SVG renders.

**light** `object` *required*

The colorway for light surfaces.

**child attributes**

**png\_1x** `string` *required*

Raster fallback at the shape's native size.

**png\_2x** `string` *required*

Raster fallback at double density.

**png\_4x** `string` *required*

Raster fallback at quadruple density.

**svg** `string` *required*

The vector file. Prefer this everywhere SVG renders.

**installment\_count** `integer | null` *required*

Installment methods only: how many payments the charge splits into. Data, not copy — compose and translate the label client-side.

Example: `42`

**payment\_method\_type** `string` *required*

The payment method type identifier, e.g. `card`, `klarna`, `apple_pay`.

**payment\_method** `object | null` *required*

The tokenized payment method reference used for this payment. Null if no token was used.

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

**fingerprint** `string | null` *required*

A stable identifier for the underlying card. Two payment methods with the same
fingerprint are the same card. Null if not available.

**last4** `string | null` *required*

The last four digits of the card number. Null if not available.

Example: `4242`

**created\_at** `string\<date-time\>` *required*

The datetime the payment token was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the payment token.

Example: `payt_xxxxxxxxxxxxx`

**payment\_method\_type** `PaymentMethodTypes` *required*

The payment method type of the payment method

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**payment\_method\_type** `PaymentMethodTypes | null` *required*

The type of payment instrument used for this payment (e.g., card, Cash App, iDEAL, Klarna, crypto). Null when the processor does not supply a type.

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**payments\_failed** `integer | null` *required*

The number of failed payment attempts for the payment.

Example: `42`

**plan** `object | null` *required*

The plan attached to this payment.

**child attributes**

**id** `string` *required*

The unique identifier for the plan.

Example: `plan_xxxxxxxxxxxxx`

**internal\_notes** `string | null` *required*

A personal description or notes section for the business.

**metadata** `object | null` *required*

Custom key-value pairs stored on the plan. Included in webhook payloads for payment and membership events. Max 50 keys, 100 chars per key, 500 chars per string value. The reserved keys `custom_cta` and `custom_cta_url`, when set, override the product's checkout call to action for this plan.

**product** `object | null` *required*

The product this payment was made for

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**metadata** `object | null` *required*

Custom key-value pairs stored on the product and included in payment and
membership webhook payloads. Max 50 keys, 100 characters per key, 500
characters per string value.

**route** `string` *required*

URL slug in the product's public link, e.g. `ceramic-coating-package` in whop.com/company/ceramic-coating-package.

Example: `ceramic-coating-package`

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`

**promo\_code** `object | null` *required*

The promo code used for this payment.

**child attributes**

**amount\_off** `number` *required*

The discount amount. Interpretation depends on promo\_type: if 'percentage', this is the percentage (e.g., 20 means 20% off); if 'flat\_amount', this is dollars off (e.g., 10.00 means \$10.00 off).

Example: `6.9`

**base\_currency** `Currencies` *required*

The monetary currency of the promo code.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**code** `string | null` *required*

The specific code used to apply the promo at checkout.

**id** `string` *required*

The unique identifier for the promo code.

Example: `promo_xxxxxxxxxxxx`

**number\_of\_intervals** `integer | null` *required*

The number of months the promo is applied for.

Example: `42`

**promo\_type** `PromoTypes` *required*

The type (% or flat amount) of the promo.

Available options: `percentage`, `flat_amount`

**recovery\_url** `string | null` *required*

Whop-hosted URL where the buyer can sign in and complete 3D Secure for an
off-session charge the bank challenged — a subscription renewal or a
saved-card payment. `null` when recovery is unavailable or you lack
`member:basic:read`.

**refundable** `boolean` *required*

True only for payments that are `paid`, have not been fully refunded, and were
processed by a payment processor that allows refunds.

**refunded\_amount** `number | null` *required*

The payment refund amount(if applicable).

Example: `6.9`

**refunded\_at** `string\<date-time\> | null` *required*

When the payment was refunded (if applicable).

Example: `2023-12-01T05:00:00.401Z`

**refunds** `array\<object\>` *required*

The refunds issued against this payment, newest first, including failed and canceled refund attempts. Limited to the 100 most recent.

**child attributes**

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

**status** `RefundStatuses` *required*

The current processing status of the refund, such as pending, succeeded, or failed.

Available options: `pending`, `requires_action`, `succeeded`, `failed`, `canceled`

**resolutions** `array\<object\> | null` *required*

The resolution center cases opened by the customer on this payment. Null if the actor in context does not have the payment:resolution\_center\_case:read permission.

**child attributes**

**customer\_appealed** `boolean` *required*

Whether the customer has filed an appeal after the initial resolution
decision.

**customer\_response\_actions** `array\<ResolutionCenterCaseCustomerResponses\>` *required*

The list of actions currently available to the customer.

Available options: `respond`, `appeal`, `withdraw`

**due\_date** `string\<date-time\> | null` *required*

The deadline by which the next response is required. Null if no deadline is currently active. As a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the resolution.

Example: `reso_xxxxxxxxxxxxx`

**issue** `ResolutionCenterCaseIssueTypes` *required*

The category of the dispute.

Available options: `forgot_to_cancel`, `item_not_received`, `significantly_not_as_described`, `unauthorized_transaction`, `product_unacceptable`, `credit_not_processed`

**merchant\_appealed** `boolean` *required*

Whether the merchant has filed an appeal after the initial resolution
decision.

**merchant\_response\_actions** `array\<ResolutionCenterCaseMerchantResponses\>` *required*

The list of actions currently available to the merchant.

Available options: `accept`, `deny`, `request_more_info`, `appeal`, `respond`

**platform\_response\_actions** `array\<ResolutionCenterCasePlatformResponses\>` *required*

The list of actions currently available to the Whop platform for moderating this resolution.

Available options: `request_buyer_info`, `request_merchant_info`, `merchant_wins`, `merchant_refund`

**status** `ResolutionCenterCaseStatuses` *required*

The current status of the resolution case, indicating which party needs to respond or if the case is closed.

Available options: `merchant_response_needed`, `customer_response_needed`, `merchant_info_needed`, `customer_info_needed`, `under_platform_review`, `customer_won`, `merchant_won`, `customer_withdrew`

**retryable** `boolean` *required*

True when the payment is `open`, its processor can be charged again, its
membership is `active`, `trialing`, or `past_due` (or it is a failed initial
payment on a `drafted` membership), it is not paid through an invoice link,
and its one manual retry is not used up. Used to decide if the payment can be
retried by hand. A retry can still be refused when the company cannot accept
payments.

**risk\_score** `integer | null` *required*

Whop's in-house fraud risk score for this payment, from 0 (lowest risk) to 100 (highest risk). Null when the payment has not been scored or scoring has not yet completed.

Example: `42`

**risk\_signals** `object | null` *required*

Deprecated. Always null.

**settlement\_amount** `number` *required*

The total amount charged to the customer for this payment, including taxes and after any discounts. In the currency specified by the currency field.

Example: `6.9`

**settlement\_currency** `Currencies` *required*

The three-letter ISO currency code for this payment (e.g., 'usd', 'eur').

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**settlement\_exchange\_rate** `number | null` *required*

Deprecated. Always returns null.

Example: `6.9`

**settlement\_time\_at** `string\<date-time\> | null` *required*

When this payment's funds post to the company's available balance, at midnight UTC. Known at payment time and never changes. The `financial_activity.funds_available` webhook's `posted_at` carries the same value when the settlement that clears it posts — match them to know these funds are now withdrawable.

Example: `2023-12-01T05:00:00.401Z`

**shipment** `object | null` *required*

The shipment attached to this payment.

**child attributes**

**carrier** `string | null` *required*

The shipping carrier detected for this shipment. Null until a tracking update identifies it.

Example: `usps`

**id** `string` *required*

The unique identifier for the shipment.

Example: `ship_xxxxxxxxxxxxx`

**status** `ShipmentStatuses` *required*

The current delivery status of this shipment.

Available options: `unknown`, `pre_transit`, `in_transit`, `out_for_delivery`, `delivered`, `available_for_pickup`, `return_to_sender`, `failure`, `cancelled`, `error`

**tracking\_number** `string` *required*

The carrier-assigned tracking number used to look up shipment progress.

Example: `9400111899223456789012`

**tracking\_url** `string` *required*

A customer-facing URL to track this shipment's progress.

Example: `https://track.aftership.com/9400111899223456789012`

**shipping\_address** `object | null` *required*

The shipping address provided by the customer for physical goods. Null if no shipping address was collected.

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

**sku** `string | null` *required*

The stock keeping unit of the variant this payment charged for (the first
item, on a cart), as set by the seller. Accounts with billing-reason SKU
suffixes enabled receive it stamped with why the charge happened: `-S` for a
first subscription charge, `-R` for a renewal, `-C` for a one-time purchase,
`-U` when the payment's own metadata carries `upsell: true`, `-F` when nothing
was charged. Other billing reasons carry the bare SKU. Null when the variant
has no SKU.

**status** `ReceiptStatus | null` *required*

The current lifecycle state of this payment (e.g., 'draft', 'open', 'paid', 'void').

Available options: `draft`, `open`, `authorized`, `paid`, `pending`, `uncollectible`, `unresolved`, `void`

**substatus** `FriendlyReceiptStatus` *required*

The friendly status of the payment.

Available options: `succeeded`, `requires_capture`, `pending`, `failed`, `blocked`, `past_due`, `canceled`, `price_too_low`, `uncollectible`, `refunded`, `auto_refunded`, `partially_refunded`, `dispute_warning`, `dispute_needs_response`, `dispute_warning_needs_response`, `resolution_needs_response`, `dispute_under_review`, `dispute_warning_under_review`, `resolution_under_review`, `dispute_won`, `dispute_warning_closed`, `resolution_won`, `dispute_lost`, `dispute_closed`, `resolution_lost`, `drafted`, `incomplete`, `unresolved`, `open_dispute`, `open_resolution`

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

**three\_ds\_verified** `boolean` *required*

Whether 3D Secure authentication was completed for this payment.

**total** `number | null` *required*

The total to show to the creator (excluding buyer fees).

Example: `6.9`

**updated\_at** `string\<date-time\>` *required*

The datetime the payment was last updated.

Example: `2023-12-01T05:00:00.401Z`

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

**verification\_checks** `object | null` *required*

The issuer's address and card security code check results for this payment. Null when the processor returned none.

**child attributes**

**address\_line1** `string | null` *required*

Whether the billing street address the customer entered matched the address
the issuer has on file.

**authorization\_code** `string | null` *required*

The card issuer's authorization code for this charge. Null when the processor
did not return one.

**card\_holder\_name** `string | null` *required*

Whether the cardholder name the customer entered matched the name the issuer
has on file.

**card\_security\_code** `string | null` *required*

Whether the CVV / CVC the customer entered matched the card.

**zip\_code** `string | null` *required*

Whether the billing postal code the customer entered matched the postal code the issuer has on file.

**voidable** `boolean` *required*

True when the payment can be voided or canceled. The request is rejected if
the payment is no longer eligible.


## Related topics

- [Payments](https://docs.whop.com/elements/latest/payments/overview.md)
- [Payments API](https://docs.whop.com/api-reference/payments/payment.md)
- [Accept Payments](https://docs.whop.com/developer/guides/accept-payments.md)
