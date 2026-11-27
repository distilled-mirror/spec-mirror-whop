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

# Payment Method

```json Example theme={null}
{
	"created_at": "2023-12-01T05:00:00.401Z",
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
	"id": "payt_xxxxxxxxxxxxx",
	"payment_method_type": "acss_debit",
	"typename": "BasePaymentMethod"
}
```

This object is one of the following variants, distinguished by `typename`:

### Base Payment Method

**created\_at** `string\<date-time\>` *required*

The time of the event in ISO 8601 UTC format with millisecond precision

Example: `2023-12-01T05:00:00.401Z`

**icons** `object` *required*

Every rendition of the icon to display this payment method with. A saved card carries its brand's icon (Visa, Mastercard, ...) rather than the generic card art.

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

**id** `string` *required*

Example: `payt_xxxxxxxxxxxxx`

**payment\_method\_type** `PaymentMethodTypes` *required*

The type of payment instrument stored on file (e.g., card, us\_bank\_account, cashapp, ideal, sepa\_debit).

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**typename** `string` *required*

The typename of this object

### Card Payment Method

**card** `object` *required*

The card-specific details for this payment method, including brand, last four digits, and expiration.

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

**expired** `boolean` *required*

Whether the card is past its expiration month. An expired card cannot take a
new charge.

**fingerprint** `string | null` *required*

A stable identifier for the underlying card. Two payment methods with the same
fingerprint are the same card. Null if not available.

**funding\_type** `CardFundingTypes | null` *required*

How the card is funded by the issuer. Null if the funding type could not be determined.

Available options: `credit`, `debit`, `prepaid`

**last4** `string | null` *required*

The last four digits of the card number. Null if not available.

Example: `4242`

**three\_ds\_verified** `boolean` *required*

Whether this card was verified with 3D Secure, either when it was saved or on a payment that used it.

**created\_at** `string\<date-time\>` *required*

The time of the event in ISO 8601 UTC format with millisecond precision

Example: `2023-12-01T05:00:00.401Z`

**has\_payer\_document** `boolean` *required*

Whether this card has the payer identity document required by its payment
provider.

**icons** `object` *required*

Every rendition of the icon to display this payment method with. A saved card carries its brand's icon (Visa, Mastercard, ...) rather than the generic card art.

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

**id** `string` *required*

**payment\_method\_type** `PaymentMethodTypes` *required*

The type of payment instrument stored on file (e.g., card, us\_bank\_account, cashapp, ideal, sepa\_debit).

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**typename** `string` *required*

The typename of this object

### Us Bank Account Payment Method

**created\_at** `string\<date-time\>` *required*

The time of the event in ISO 8601 UTC format with millisecond precision

Example: `2023-12-01T05:00:00.401Z`

**icons** `object` *required*

Every rendition of the icon to display this payment method with. A saved card carries its brand's icon (Visa, Mastercard, ...) rather than the generic card art.

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

**id** `string` *required*

**payment\_method\_type** `PaymentMethodTypes` *required*

The type of payment instrument stored on file (e.g., card, us\_bank\_account, cashapp, ideal, sepa\_debit).

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**typename** `string` *required*

The typename of this object

**us\_bank\_account** `object` *required*

The bank account-specific details for this payment method, including bank name and last four digits.

**child attributes**

**account\_type** `string | null` *required*

The type of bank account (e.g., checking, savings).

Example: `checking`

**bank\_name** `string | null` *required*

The name of the financial institution holding the account.

Example: `Chase`

**last4** `string | null` *required*

The last four digits of the bank account number.

Example: `6789`

### Cashapp Payment Method

**cashapp** `object` *required*

The Cash App-specific details for this payment method, including cashtag and buyer ID.

**child attributes**

**buyer\_id** `string | null` *required*

The unique and immutable identifier assigned by Cash App to the buyer. Null if not available.

Example: `BUYER_abc123`

**cashtag** `string | null` *required*

The public cashtag handle of the buyer on Cash App. Null if not available.

Example: `$jacksmith`

**created\_at** `string\<date-time\>` *required*

The time of the event in ISO 8601 UTC format with millisecond precision

Example: `2023-12-01T05:00:00.401Z`

**icons** `object` *required*

Every rendition of the icon to display this payment method with. A saved card carries its brand's icon (Visa, Mastercard, ...) rather than the generic card art.

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

**id** `string` *required*

**payment\_method\_type** `PaymentMethodTypes` *required*

The type of payment instrument stored on file (e.g., card, us\_bank\_account, cashapp, ideal, sepa\_debit).

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**typename** `string` *required*

The typename of this object

### Ideal Payment Method

**created\_at** `string\<date-time\>` *required*

The time of the event in ISO 8601 UTC format with millisecond precision

Example: `2023-12-01T05:00:00.401Z`

**icons** `object` *required*

Every rendition of the icon to display this payment method with. A saved card carries its brand's icon (Visa, Mastercard, ...) rather than the generic card art.

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

**id** `string` *required*

**ideal** `object` *required*

The iDEAL-specific details for this payment method, including bank name and BIC.

**child attributes**

**bank** `string | null` *required*

The name of the customer's bank used for the iDEAL transaction. Null if not available.

Example: `ing`

**bic** `string | null` *required*

The Bank Identifier Code (BIC/SWIFT) of the customer's bank. Null if not available.

Example: `INGBNL2A`

**payment\_method\_type** `PaymentMethodTypes` *required*

The type of payment instrument stored on file (e.g., card, us\_bank\_account, cashapp, ideal, sepa\_debit).

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**typename** `string` *required*

The typename of this object

### Sepa Debit Payment Method

**created\_at** `string\<date-time\>` *required*

The time of the event in ISO 8601 UTC format with millisecond precision

Example: `2023-12-01T05:00:00.401Z`

**icons** `object` *required*

Every rendition of the icon to display this payment method with. A saved card carries its brand's icon (Visa, Mastercard, ...) rather than the generic card art.

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

**id** `string` *required*

**payment\_method\_type** `PaymentMethodTypes` *required*

The type of payment instrument stored on file (e.g., card, us\_bank\_account, cashapp, ideal, sepa\_debit).

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**sepa\_debit** `object` *required*

The SEPA Direct Debit-specific details for this payment method, including bank code and last four IBAN digits.

**child attributes**

**bank\_code** `string | null` *required*

The bank code of the financial institution associated with this SEPA account. Null if not available.

Example: `37040044`

**branch\_code** `string | null` *required*

The branch code of the financial institution associated with this SEPA account. Null if not available.

Example: `0532`

**country** `string | null` *required*

The two-letter ISO country code where the bank account is located. Null if not available.

Example: `DE`

**last4** `string | null` *required*

The last four digits of the IBAN associated with this SEPA account. Null if not available.

Example: `3000`

**typename** `string` *required*

The typename of this object


## Related topics

- [Payments](https://docs.whop.com/elements/latest/payments/overview.md)
- [Payment](https://docs.whop.com/api-reference/legacy/payments/payment.md)
- [Save Payment Methods](https://docs.whop.com/developer/guides/save-payment-methods.md)
