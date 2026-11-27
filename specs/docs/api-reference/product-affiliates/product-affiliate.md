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

# Product Affiliates API

> A Product Affiliate represents a user's enrollment in a product's affiliate program. An enrollment can exist before the user makes a referral or earns a reward.

Use the Product Affiliates API to list product affiliates for an account, search by affiliate name or username, and narrow the results by product, enrollment status, or signup date.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Product Affiliates](https://docs.whop.com/api-reference/product-affiliates/list-product-affiliates.md) | GET `/product_affiliates` |

## Attributes

**id** `string` *required*

Product affiliate ID, prefixed `apaf_`.

**created\_at** `string` *required*

When the affiliate enrollment was created, as an ISO 8601 timestamp. Includes
automatically created enrollments.

**product** `object` *required*

The product the affiliate is enrolled to promote.

> [accordion] Properties
>
> **id** `string` *required*
>
> Product ID, prefixed `prod_`.
>
> **title** `string` *required*
>
> Product display name.

**referred\_users\_count** `integer` *required*

Number of distinct users referred by the affiliate to this product.

**status** `string` *required*

Whether the affiliate's enrollment is active or archived.

Available options: `active`, `archived`

**total\_reward\_amount\_usd** `object` *required*

Net rewards earned by the affiliate for this product after fees and reversals, in USD.

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

**user** `object` *required*

The user enrolled as an affiliate.

> [accordion] Properties
>
> **id** `string` *required*
>
> User ID, prefixed `user_`.
>
> **badges** `object[]` *required*
>
> Public badges for the user, in display order. Empty when the user has no badges or is suspended.
>
> > [accordion] Properties
> >
> > **type** `string` *required*
> >
> > Badge category. `whop_team` identifies a Whop team member. `verified_partner` identifies a verified member of the Whop Partners program.
> >
> > Available options: `whop_team`, `verified_partner`
>
> **email** `string | null` *required*
>
> The affiliate's email address. Requires the `member:email:read` scope; `null`
> without it.
>
> **name** `string | null` *required*
>
> Display name.
>
> **profile\_picture** `object` *required*
>
> Avatar wrapper. Its `url` is always present, and is a generated placeholder when the user set no picture.
>
> > [accordion] Properties
> >
> > **url** `string` *required*
> >
> > Avatar image URL. Always present. It is a generated placeholder when the user set no picture.
>
> **username** `string` *required*
>
> Public username.

```json ProductAffiliate theme={null}
{
	"id": "apaf_xxxxxxxxxxxxxx",
	"status": "active",
	"product": {
		"id": "prod_xxxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	},
	"user": {
		"id": "user_xxxxxxxxxxxxxx",
		"name": "Dana Whitfield",
		"username": "danawhitfield",
		"email": "dana@shinetime.example",
		"profile_picture": {
			"url": "https://ui-avatars.com/api/?name=Dana+Whitfield"
		},
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"referred_users_count": 3,
	"total_reward_amount_usd": {
		"amount": "147.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"created_at": "2026-08-20T11:00:00.000Z"
}
```


## Related topics

- [List Product Affiliates](https://docs.whop.com/api-reference/product-affiliates/list-product-affiliates.md)
- [Products API](https://docs.whop.com/api-reference/products/product.md)
- [Update product](https://docs.whop.com/api-reference/legacy/products/update-product.md)
