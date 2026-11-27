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

# Checkout Configuration

> A checkout configuration is a reusable configuration for a checkout, including the plan, affiliate, and custom metadata. Payments and memberships created from a checkout session inherit its metadata.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/checkout-configurations/checkout-configuration.md),
> the versioned API that new integrations should build on. This page stays fully
> supported.

```json Example theme={null}
{
	"account_id": "<string>",
	"affiliate_code": "<string>",
	"company_id": "<string>",
	"currency": "usd",
	"id": "ch_xxxxxxxxxxxxxxx",
	"metadata": {},
	"mode": "payment",
	"payment_method_configuration": {
		"disabled": ["acss_debit"],
		"enabled": ["acss_debit"],
		"include_platform_defaults": true
	},
	"plan": {
		"adaptive_pricing_enabled": true,
		"billing_period": 42,
		"currency": "usd",
		"expiration_days": 42,
		"id": "plan_xxxxxxxxxxxxx",
		"initial_price": 6.9,
		"plan_type": "renewal",
		"release_method": "buy_now",
		"renewal_price": 6.9,
		"three_ds_level": "mandate_challenge",
		"trial_period_days": 42,
		"visibility": "visible"
	},
	"purchase_url": "<string>",
	"redirect_url": "<string>"
}
```

**account\_id** `string` *required*

The ID of the account to use for the checkout configuration

**affiliate\_code** `string | null` *required*

The affiliate code to use for the checkout configuration

**company\_id** `string` *required*

The ID of the account to use for the checkout configuration

**currency** `Currencies | null` *required*

The currency to use for the configuration when in 'setup' mode. This is used to target which currency specific payment methods are available. If not provided, it will default to 'usd' when in setup mode.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**id** `string` *required*

The unique identifier for the checkout session.

Example: `ch_xxxxxxxxxxxxxxx`

**metadata** `object | null` *required*

The metadata to use for the checkout configuration

**mode** `CheckoutModes` *required*

The mode of the checkout session.

Available options: `payment`, `setup`

**payment\_method\_configuration** `object | null` *required*

The explicit payment method configuration for the session, if any. This currently only works in 'setup' mode. Use the plan's payment\_method\_configuration for payment method.

**child attributes**

**disabled** `array\<PaymentMethodTypes\>` *required*

An array of payment method identifiers that are explicitly disabled. Only applies if the include\_platform\_defaults is true.

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**enabled** `array\<PaymentMethodTypes\>` *required*

An array of payment method identifiers that are explicitly enabled. This means these payment methods will be shown on checkout. Example use case is to only enable a specific payment method like cashapp, or extending the platform defaults with additional methods.

Available options: `acss_debit`, `addi`, `affirm`, `afterpay_clearpay`, `alipay`, `alipayhk`, `alma`, `amazon_pay`, `apple`, `apple_pay`, `au_bank_transfer`, `au_becs_debit`, `bacs_debit`, `bancolombia`, `bancontact`, `bank_wire`, `billie`, `bizum`, `blik`, `boleto`, `bre_b`, `ca_bank_transfer`, `capchase_pay`, `card`, `card_present`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `cashapp`, `claritypay`, `coinbase`, `crypto`, `crypto_subscription`, `custom`, `customer_balance`, `demo_pay`, `duitnow_qr`, `efecty`, `eps`, `eu_bank_transfer`, `fpx`, `flex_pay`, `gb_bank_transfer`, `gcash`, `giropay`, `google_pay`, `gopay`, `grabpay`, `id_bank_transfer`, `ideal`, `interac`, `international_bank_wire`, `kakao_pay`, `klarna`, `klarna_pay_now`, `konbini`, `kr_card`, `kr_market`, `kriya`, `kueski`, `link`, `mb_way`, `m_pesa`, `mercado_pago`, `mercado_pago_ar`, `mercado_pago_mx`, `mobilepay`, `modo`, `mondu`, `multibanco`, `naver_pay`, `nequi`, `netbanking`, `ng_bank`, `ng_bank_transfer`, `ng_card`, `ng_market`, `ng_ussd`, `ng_wallet`, `nupay`, `nz_bank_account`, `oney`, `oney_3x`, `oney_4x`, `opay`, `oxxo`, `p24`, `pago_efectivo`, `pse`, `pay_by_bank`, `payco`, `paynow`, `paypal`, `paypal_express`, `paypay`, `payto`, `payto_email`, `payto_phone`, `payto_bank_account`, `pix`, `pix_automatico`, `platform_balance`, `promptpay`, `qris`, `rapipago`, `rechnung`, `revolut_pay`, `samsung_pay`, `satispay`, `scalapay`, `sencillito`, `sepa_debit`, `sequra`, `servipag`, `sezzle`, `shop_pay`, `shopeepay`, `sofort`, `south_korea_market`, `spei`, `splitit`, `sunbit`, `swish`, `tabby`, `tamara`, `touch_n_go`, `twint`, `upi`, `us_bank_account`, `us_bank_transfer`, `venmo`, `verve`, `vipps`, `webpay`, `wechat_pay`, `whop_pay`, `yape`, `zip`, `coinflow`, `unknown`

**include\_platform\_defaults** `boolean` *required*

Whether Whop's platform default payment method enablement settings are included in this configuration. The full list of default payment methods can be found in the documentation at docs.whop.com/payments.

**plan** `object | null` *required*

The plan to use for the checkout configuration

**child attributes**

**adaptive\_pricing\_enabled** `boolean` *required*

Whether the creator has turned on adaptive pricing for this plan. Raw setting
— does not check processor compatibility or feature flags.

**billing\_period** `integer | null` *required*

Number of days between recurring charges, such as 30 for monthly or 365 for annual. `null` for one-time plans.

Example: `42`

**currency** `Currencies` *required*

The currency used for all prices on this plan (e.g., 'usd', 'eur'). All monetary amounts on the plan are denominated in this currency.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**expiration\_days** `integer | null` *required*

Access duration in days for expiration-based plans, such as 365 for a one-year pass.

Example: `42`

**id** `string` *required*

The unique identifier for the plan.

Example: `plan_xxxxxxxxxxxxx`

**initial\_price** `number` *required*

The initial purchase price in the plan's base\_currency (e.g., 49.99 for \$49.99). For one-time plans, this is the full price. For renewal plans, this is charged on top of the first renewal\_price.

Example: `6.9`

**plan\_type** `PlanTypes` *required*

The billing model for this plan: 'renewal' for recurring subscriptions or 'one\_time' for single payments.

Available options: `renewal`, `one_time`

**release\_method** `ReleaseMethod` *required*

Sales method for this plan: `buy_now` for immediate purchase or `waitlist` for waitlist-based access.

Available options: `buy_now`, `waitlist`

**renewal\_price** `number` *required*

The recurring price charged every billing\_period in the plan's base\_currency (e.g., 9.99 for \$9.99/period). Zero for one-time plans.

Example: `6.9`

**three\_ds\_level** `PlanThreeDsLevels | null` *required*

The 3D Secure behavior for this plan. Null means the plan inherits the account default.

Available options: `mandate_challenge`, `frictionless`

**trial\_period\_days** `integer | null` *required*

Free trial days before first renewal charge. `null` if no trial is configured or the user has already used a trial for this plan.

Example: `42`

**visibility** `Visibility` *required*

Controls whether the plan is visible to customers. When set to 'hidden', the plan is only accessible via direct link.

Available options: `visible`, `hidden`, `archived`, `quick_link`

**purchase\_url** `string` *required*

A URL you can send to customers to complete a checkout. It looks like
`/checkout/ch_xxxx/`

**redirect\_url** `string | null` *required*

The URL to redirect the user to after the checkout configuration is created


## Related topics

- [Whop CLI Commands](https://docs.whop.com/cli/commands.md)
- [Checkout Configurations API](https://docs.whop.com/api-reference/checkout-configurations/checkout-configuration.md)
- [Retrieve a Checkout Configuration](https://docs.whop.com/api-reference/checkout-configurations/retrieve-a-checkout-configuration.md)
