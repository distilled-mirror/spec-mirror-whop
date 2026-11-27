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

# Promo Code

> A promo code applies a discount to a plan during checkout. Promo codes can be percentage-based or fixed-amount, and can have usage limits and expiration dates.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/promo-codes/list-promo-codes.md), the versioned API
> that new integrations should build on. This page stays fully supported.

```json Example theme={null}
{
	"amount_off": 6.9,
	"churned_users_only": true,
	"code": "<string>",
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "<string>"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "usd",
	"duration": "forever",
	"existing_memberships_only": true,
	"expires_at": "2023-12-01T05:00:00.401Z",
	"id": "promo_xxxxxxxxxxxx",
	"new_users_only": true,
	"one_per_customer": true,
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	},
	"promo_duration_months": 42,
	"promo_type": "percentage",
	"status": "active",
	"stock": 42,
	"unlimited_stock": true,
	"uses": 42
}
```

**amount\_off** `number` *required*

The discount amount. Interpretation depends on promo\_type: if 'percentage', this is the percentage (e.g., 20 means 20% off); if 'flat\_amount', this is dollars off (e.g., 10.00 means \$10.00 off).

Example: `6.9`

**churned\_users\_only** `boolean` *required*

Restricts promo use to only users who have churned from the company before.

**code** `string | null` *required*

The specific code used to apply the promo at checkout.

**company** `object` *required*

The company for the promo code.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**title** `string` *required*

The written name of the company.

**created\_at** `string\<date-time\>` *required*

The datetime the promo code was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies` *required*

The monetary currency of the promo code.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**duration** `PromoDurations | null` *required*

The duration of the promo.

Available options: `forever`, `once`, `repeating`

**existing\_memberships\_only** `boolean` *required*

Restricts promo use to only be applied to already purchased memberships.

**expires\_at** `string\<date-time\> | null` *required*

The date/time of when the promo expires.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the promo code.

Example: `promo_xxxxxxxxxxxx`

**new\_users\_only** `boolean` *required*

Restricts promo use to only users who have never purchased from the company
before.

**one\_per\_customer** `boolean` *required*

Restricts promo use to only be applied once per customer.

**product** `object | null` *required*

The product this promo code applies to

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`

**promo\_duration\_months** `integer | null` *required*

The number of months the promo is applied for.

Example: `42`

**promo\_type** `PromoTypes` *required*

The type (% or flat amount) of the promo.

Available options: `percentage`, `flat_amount`

**status** `PromoCodeStatus` *required*

Indicates if the promo code is live or disabled.

Available options: `active`, `inactive`, `archived`

**stock** `integer` *required*

The quantity limit on the number of uses.

Example: `42`

**unlimited\_stock** `boolean` *required*

Whether or not the promo code has unlimited stock.

**uses** `integer` *required*

The amount of times the promo codes has been used.

Example: `42`


## Related topics

- [Whop CLI Commands](https://docs.whop.com/cli/commands.md)
- [Promo Codes API](https://docs.whop.com/api-reference/promo-codes/promo-code.md)
- [Delete Promo Code](https://docs.whop.com/api-reference/promo-codes/delete-promo-code.md)
