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

# Promo Codes API

> A Promo Code is a discount a buyer enters at checkout, either a percentage or a fixed amount off.

It belongs to an account, can be limited to one product, and controls who can redeem it, how many times, until when, and for how many billing periods the discount lasts.

Use the Promo Codes API to create codes for an account, list and retrieve them, deactivate a code to pause redemptions or activate it again, and archive codes you no longer offer.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Promo Codes](https://docs.whop.com/api-reference/promo-codes/list-promo-codes.md) | GET `/promo_codes` |
| [Retrieve Promo Code](https://docs.whop.com/api-reference/promo-codes/retrieve-promo-code.md) | GET `/promo_codes/{id}` |
| [Create Promo Code](https://docs.whop.com/api-reference/promo-codes/create-promo-code.md) | POST `/promo_codes` |
| [Activate Promo Code](https://docs.whop.com/api-reference/promo-codes/activate-promo-code.md) | POST `/promo_codes/{id}/activate` |
| [Deactivate Promo Code](https://docs.whop.com/api-reference/promo-codes/deactivate-promo-code.md) | POST `/promo_codes/{id}/deactivate` |
| [Delete Promo Code](https://docs.whop.com/api-reference/promo-codes/delete-promo-code.md) | DELETE `/promo_codes/{id}` |

## Attributes

**id** `string` *required*

Promo code ID, prefixed `promo_`.

**account** `object` *required*

Account that owns the promo code.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **title** `string` *required*
>
> Account display name.

**affiliate** `object | null` *required*

Affiliate credited with referrals that use the promo code, or `null` when no affiliate is attached.

> [accordion] Properties
>
> **id** `string` *required*
>
> Affiliate ID, prefixed `aff_`.
>
> **user** `object` *required*
>
> User who is the affiliate.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Affiliate user ID, prefixed `user_`.
> >
> > **name** `string | null` *required*
> >
> > Display name of the affiliate.
> >
> > **profile\_pic** `string` *required*
> >
> > Affiliate avatar URL, including the generated fallback when no avatar is set.
> >
> > **username** `string | null` *required*
> >
> > Public username of the affiliate.

**amount\_off** `number` *required*

Discount amount. Percentage discounts are represented as a decimal fraction.

**churned\_users\_only** `boolean` *required*

Whether the promo code is restricted to churned customers.

**code** `string | null` *required*

Code entered at checkout.

**created\_at** `string` *required*

When the promo code was created, as an ISO 8601 timestamp.

**currency** `string` *required*

Currency used for a fixed-amount discount.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**duration** `string` *required*

How long the discount applies.

Available options: `forever`, `once`, `repeating`

**existing\_memberships\_only** `boolean` *required*

Whether the promo code applies only to existing memberships.

**expires\_at** `string | null` *required*

When the promo code expires, as an ISO 8601 timestamp.

**metadata** `object` *required*

Custom key-value metadata stored on the promo code.

**new\_users\_only** `boolean` *required*

Whether the promo code is restricted to new customers.

**one\_per\_customer** `boolean` *required*

Whether each customer may redeem the promo code only once.

**product** `object | null` *required*

Product the promo code is restricted to, or `null` when it is not product-scoped.

> [accordion] Properties
>
> **id** `string` *required*
>
> Product ID, prefixed `prod_`.
>
> **title** `string` *required*
>
> Product display name.

**promo\_duration\_months** `integer | null` *required*

Billing intervals the discount applies to.

**promo\_type** `string` *required*

Whether the discount is percentage-based or a fixed amount.

Available options: `percentage`, `flat_amount`

**status** `string` *required*

Promo code lifecycle status.

Available options: `active`, `inactive`, `archived`

**stock** `integer` *required*

Maximum uses when stock is limited.

**unlimited\_stock** `boolean` *required*

Whether the promo code has no redemption limit.

**updated\_at** `string` *required*

When the promo code was updated, as an ISO 8601 timestamp.

**uses** `integer` *required*

Memberships that used the promo code.

```json PromoCode theme={null}
{
	"id": "promo_xxxxxxxxxxxxxx",
	"code": "SUMMER25",
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"product": {
		"id": "prod_xxxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	},
	"affiliate": {
		"id": "aff_xxxxxxxxxxxxxx",
		"user": {
			"id": "user_xxxxxxxxxxxxxx",
			"username": "danawhitfield",
			"name": "Dana Whitfield",
			"profile_pic": "https://ui-avatars.com/api/?name=Dana+Whitfield"
		}
	},
	"status": "active",
	"promo_type": "percentage",
	"amount_off": 0.25,
	"currency": "usd",
	"duration": "repeating",
	"promo_duration_months": 3,
	"new_users_only": true,
	"churned_users_only": false,
	"existing_memberships_only": false,
	"one_per_customer": true,
	"unlimited_stock": false,
	"stock": 200,
	"uses": 37,
	"expires_at": "2026-12-31T23:59:59.000Z",
	"metadata": {},
	"created_at": "2026-09-01T12:00:00.000Z",
	"updated_at": "2026-09-01T12:00:00.000Z"
}
```


## Related topics

- [Promo Code](https://docs.whop.com/api-reference/legacy/promo-codes/promo-code.md)
- [Delete Promo Code](https://docs.whop.com/api-reference/promo-codes/delete-promo-code.md)
- [Delete promo code](https://docs.whop.com/api-reference/legacy/promo-codes/delete-promo-code.md)
