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

# Payment Quotes API

> A Payment Quote prices a purchase exactly as a payment for it will be charged: the same line pricing, promo code, and tax, for a buyer located by the address, country, or IP address you send.

A quote is priced once, can be consumed by one payment, and expires.

Use the Payment Quotes API to show a buyer the total, tax, and currency before they pay, then pass the quote's `id` as `quote_id` when you create the payment to charge exactly that quote. Quote again whenever the cart, promo code, or address changes.

## Endpoints

| Endpoint | Request |
| - | - |
| [Retrieve Payment Quote](https://docs.whop.com/api-reference/payment-quotes/retrieve-payment-quote.md) | GET `/payment_quotes/{id}` |
| [Create Payment Quote](https://docs.whop.com/api-reference/payment-quotes/create-payment-quote.md) | POST `/payment_quotes` |

## Attributes

**id** `string` *required*

Payment quote ID, prefixed `pq_`.

**account\_id** `string` *required*

The account the purchase is priced for, prefixed `biz_`.

**address** `object | null` *required*

The billing address the purchase was priced with, or null. Where tax was calculated when no shipping address was given, and the address the registration belongs to.

> [accordion] Properties
>
> **city** `string | null` *required*
>
> The city.
>
> **country** `string | null` *required*
>
> The ISO 3166-1 alpha-2 country code.
>
> **line1** `string | null` *required*
>
> The first street address line.
>
> **line2** `string | null` *required*
>
> The second street address line.
>
> **name** `string | null` *required*
>
> The name on the address.
>
> **postal\_code** `string | null` *required*
>
> The postal or ZIP code.
>
> **state** `string | null` *required*
>
> The state, province or region.

**base\_currency** `string` *required*

Three-letter ISO 4217 code of the variants' own currency, lowercase. Equal to
`currency` when nothing was converted.

**base\_total** `object` *required*

`total` in `base_currency` at `exchange_rate`: the variants' own prices after the promo code, plus `tax_amount` converted at `exchange_rate` when tax is added on top. Equal to `total` when nothing was converted. The payment charges `total`, in `currency`.

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

**created\_at** `string` *required*

When the quote was priced, as an ISO 8601 timestamp.

**currency** `string` *required*

Three-letter ISO 4217 currency code the purchase is priced and charged in,
lowercase: the variants' own currency, or the `presentment_currency` it was
converted into.

**discount** `object` *required*

What the promo code takes off. Zero without a code.

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

**exchange\_rate** `string | null` *required*

How many units of `currency` one unit of `base_currency` buys, as a decimal
string such as `"5.4321"`: the rate the variants' prices were converted at,
fixed until `expires_at`, and the rate a payment consuming the quote is
charged at. A string, like money amounts, so no float rounds it in transit.
Null when nothing was converted.

**expires\_at** `string` *required*

When the quote stops being chargeable, as an ISO 8601 timestamp. Quote again
after it.

**line\_items** `object[]` *required*

One entry per variant, in the order given, each carrying its own tax.

> [accordion] Properties
>
> **discount** `object` *required*
>
> The promo code's share on this line. Zero without a code.
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
> **quantity** `integer` *required*
>
> How many units of the variant the line covers.
>
> **subtotal** `object` *required*
>
> The line's price before the promo code's share and tax; `subtotal` minus `discount` is what the line comes to after the discount. The lines' subtotals sum to the quote's `subtotal`.
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
> **tax\_amount** `object` *required*
>
> The tax on this line. Zero when `tax_status` is not `calculated`.
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
> **total** `object` *required*
>
> `subtotal` minus `discount`, plus `tax_amount` when tax is added on top, or without it when the price already includes it.
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
> **variant\_id** `string | null` *required*
>
> The variant this line prices, prefixed `plan_`. Null when the purchase describes the variant by `variant` and no variant with those attributes exists yet: the payment that consumes the quote creates it, and the line names it from then on.

**located\_by** `string | null` *required*

Which location tax was calculated for: `shipping_address` when it carries a country, else the billing `address` when it does, else the `presentment_country` sent, else the buyer's `ip_address`. A quote located by `presentment_country` or `ip_address` is a preview that estimates tax: a payment cannot use it, so quote again with the buyer's address to pay. Null when nothing in the request located the buyer, which only a seller that collects no tax on this purchase is quoted without; `tax_status` is then `not_applicable`.

Available options: `shipping_address`, `address`, `presentment_country`, `ip_address`

**payment\_id** `string | null` *required*

The payment holding this quote, prefixed `pay_`, or null while it is unspent.
A declined payment keeps its quote and can be retried; check that payment's
`status`.

**promo\_code\_id** `string | null` *required*

The promo code the quote applied, prefixed `promo_`, or null.

**recommended\_currencies** `string[]` *required*

Lowercase ISO 4217 codes to offer the buyer in a currency picker, most
relevant first: the currency of the `presentment_country` sent, else of the
country the buyer's `ip_address` is in, when this purchase can be converted
into it and a payment method can collect it, then `base_currency`. Quote again
with one as `presentment_currency` to price in it. Only `base_currency` when
the quote was priced without `presentment_currency` or `presentment_country`.

**shipping\_address** `object | null` *required*

The shipping address the purchase was priced with, or null. When present it is where tax was calculated.

> [accordion] Properties
>
> **city** `string | null` *required*
>
> The city.
>
> **country** `string | null` *required*
>
> The ISO 3166-1 alpha-2 country code.
>
> **line1** `string | null` *required*
>
> The first street address line.
>
> **line2** `string | null` *required*
>
> The second street address line.
>
> **name** `string | null` *required*
>
> The name on the address.
>
> **postal\_code** `string | null` *required*
>
> The postal or ZIP code.
>
> **state** `string | null` *required*
>
> The state, province or region.

**subtotal** `object` *required*

The price of every line before the promo code, tax and fees.

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

**tax\_amount** `object` *required*

The tax a payment consuming this quote charges. Zero unless `tax_status` is `calculated`.

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

**tax\_behavior** `string | null` *required*

Whether tax is added on top of the price (`exclusive`) or already inside it (`inclusive`). Null when no tax was calculated.

Available options: `inclusive`, `exclusive`

**tax\_ids** `object[]` *required*

The buyer's tax registration the quote priced as a business purchase. Empty for a consumer purchase.

> [accordion] Properties
>
> **type** `string` *required*
>
> The registration's type, such as `eu_vat`.
>
> Available options: `ad_nrt`, `ao_tin`, `ar_cuit`, `al_tin`, `am_tin`, `aw_tin`, `au_abn`, `au_arn`, `eu_vat`, `az_tin`, `bs_tin`, `bh_vat`, `bd_bin`, `bb_tin`, `by_tin`, `bj_ifu`, `bo_tin`, `ba_tin`, `br_cnpj`, `br_cpf`, `bg_uic`, `bf_ifu`, `kh_tin`, `cm_niu`, `ca_bn`, `ca_gst_hst`, `ca_pst_bc`, `ca_pst_mb`, `ca_pst_sk`, `ca_qst`, `cv_nif`, `cl_tin`, `cn_tin`, `co_nit`, `cd_nif`, `cr_tin`, `hr_oib`, `do_rcn`, `ec_ruc`, `eg_tin`, `sv_nit`, `et_tin`, `eu_oss_vat`, `ge_vat`, `gh_tin`, `de_stn`, `gb_vat`, `gn_nif`, `hk_br`, `hu_tin`, `is_vat`, `in_gst`, `id_npwp`, `il_vat`, `jp_cn`, `jp_rn`, `jp_trn`, `kz_bin`, `ke_pin`, `kg_tin`, `la_tin`, `li_uid`, `li_vat`, `my_frp`, `my_itn`, `my_sst`, `mr_nif`, `mx_rfc`, `md_vat`, `me_pib`, `ma_vat`, `np_pan`, `nz_gst`, `ng_tin`, `mk_vat`, `no_vat`, `no_voec`, `om_vat`, `pe_ruc`, `ph_tin`, `pl_nip`, `ro_tin`, `ru_inn`, `ru_kpp`, `sa_vat`, `sn_ninea`, `rs_pib`, `sg_gst`, `sg_uen`, `si_tin`, `za_vat`, `kr_brn`, `es_cif`, `ch_uid`, `ch_vat`, `tw_vat`, `tj_tin`, `tz_vat`, `th_vat`, `tr_tin`, `ug_tin`, `ua_vat`, `ae_trn`, `us_ein`, `uy_ruc`, `uz_tin`, `uz_vat`, `ve_rif`, `vn_tin`, `zm_tin`, `zw_tin`, `sr_fin`, `xi_vat`
>
> **value** `string` *required*
>
> The registration number as the buyer entered it, whitespace removed (`eu_vat` values also drop dots and dashes).

**tax\_status** `string` *required*

`calculated`: every line was priced and a payment may consume the quote. `not_applicable`: this seller collects no tax on this purchase, so the quote owes none and may still be consumed. `unavailable`: tax could not be priced — the provider did not answer, or this seller's tax setup cannot price a purchase here — so a payment refuses the quote; quote again, or charge without `quote_id` to have tax calculated at charge time.

Available options: `calculated`, `not_applicable`, `unavailable`

**total** `object` *required*

What the buyer pays: the subtotal less the discount, plus `tax_amount` when tax is added on top.

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

```json PaymentQuote theme={null}
{
	"id": "pq_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"currency": "usd",
	"base_currency": "usd",
	"exchange_rate": null,
	"recommended_currencies": ["usd"],
	"tax_status": "calculated",
	"tax_behavior": "exclusive",
	"subtotal": {
		"currency": "usd",
		"amount": "27.00",
		"decimals": 2,
		"display_decimals": 2
	},
	"discount": {
		"currency": "usd",
		"amount": "0.00",
		"decimals": 2,
		"display_decimals": 2
	},
	"tax_amount": {
		"currency": "usd",
		"amount": "2.23",
		"decimals": 2,
		"display_decimals": 2
	},
	"total": {
		"currency": "usd",
		"amount": "29.23",
		"decimals": 2,
		"display_decimals": 2
	},
	"base_total": {
		"currency": "usd",
		"amount": "29.23",
		"decimals": 2,
		"display_decimals": 2
	},
	"line_items": [
		{
			"variant_id": "plan_xxxxxxxxxxxxxx",
			"quantity": 1,
			"subtotal": {
				"currency": "usd",
				"amount": "27.00",
				"decimals": 2,
				"display_decimals": 2
			},
			"discount": {
				"currency": "usd",
				"amount": "0.00",
				"decimals": 2,
				"display_decimals": 2
			},
			"tax_amount": {
				"currency": "usd",
				"amount": "2.23",
				"decimals": 2,
				"display_decimals": 2
			},
			"total": {
				"currency": "usd",
				"amount": "29.23",
				"decimals": 2,
				"display_decimals": 2
			}
		}
	],
	"promo_code_id": null,
	"located_by": "address",
	"address": {
		"name": null,
		"line1": "123 Congress Ave",
		"line2": null,
		"city": "Austin",
		"state": "TX",
		"postal_code": "78701",
		"country": "US"
	},
	"shipping_address": null,
	"tax_ids": [
		{
			"type": "us_ein",
			"value": "12-3456789"
		}
	],
	"payment_id": null,
	"expires_at": "2026-10-11T19:32:20.388Z",
	"created_at": "2026-10-11T19:02:20.389Z"
}
```


## Related topics

- [Retrieve Payment Quote](https://docs.whop.com/api-reference/payment-quotes/retrieve-payment-quote.md)
- [Create Payment Quote](https://docs.whop.com/api-reference/payment-quotes/create-payment-quote.md)
- [Payments API](https://docs.whop.com/api-reference/payments/payment.md)
