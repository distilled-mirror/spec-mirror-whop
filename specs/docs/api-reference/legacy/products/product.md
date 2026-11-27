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

# Product

> A product is a digital good or service sold on Whop. Products contain plans for pricing and experiences for content delivery.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/products/product.md), the versioned API that new
> integrations should build on. This page stays fully supported.

```json Example theme={null}
{
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"route": "shine-time-auto-detailing",
		"title": "Shine Time Auto Detailing"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"custom_cta": "get_access",
	"custom_cta_url": "https://shinetime.example/book",
	"custom_statement_descriptor": "SHINE",
	"description": "Two-stage paint correction, a three-year ceramic coating, and sealed wheel faces.",
	"external_identifier": "ext_prod_12345",
	"gallery_images": [
		{
			"content_type": "image/jpeg",
			"id": "<string>",
			"url": "https://media.whop.com/abc123/optimized.jpg"
		}
	],
	"global_affiliate_percentage": 6.9,
	"global_affiliate_status": "enabled",
	"headline": "A showroom finish that lasts three years",
	"id": "prod_xxxxxxxxxxxxx",
	"member_affiliate_percentage": 6.9,
	"member_affiliate_status": "enabled",
	"member_count": 42,
	"metadata": {},
	"owner_user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	},
	"product_tax_code": {
		"id": "ptc_xxxxxxxxxxxxxx",
		"name": "Services",
		"product_type": "physical"
	},
	"published_reviews_count": 42,
	"route": "ceramic-coating-package",
	"title": "Ceramic Coating Package",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"verified": true,
	"visibility": "visible"
}
```

**company** `object` *required*

The company this product belongs to.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**route** `string` *required*

URL slug for the account's store page. For example, `shine-time-auto-detailing` in whop.com/shine-time-auto-detailing.

Example: `shine-time-auto-detailing`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**created\_at** `string\<date-time\>` *required*

The datetime the product was created.

Example: `2023-12-01T05:00:00.401Z`

**custom\_cta** `CustomCtas` *required*

Call-to-action button label shown on the product purchase page.

Available options: `get_access`, `join`, `order_now`, `shop_now`, `call_now`, `donate_now`, `contact_us`, `sign_up`, `subscribe`, `purchase`, `get_offer`, `apply_now`, `complete_order`

**custom\_cta\_url** `string | null` *required*

An optional URL that the call-to-action button links to instead of the default checkout flow. Null if no custom URL is set.

Example: `https://shinetime.example/book`

**custom\_statement\_descriptor** `string | null` *required*

Custom bank statement descriptor for product purchases. Maximum 22 characters, including required `WHOP*` prefix.

Example: `SHINE`

**description** `string | null` *required*

A brief summary of what the product offers, displayed on product pages and search results.

Example: `Two-stage paint correction, a three-year ceramic coating,…`

**external\_identifier** `string | null` *required*

External identifier for the product. Providing it on a product creation endpoint updates the existing product with this identifier instead of creating a new one.

Example: `ext_prod_12345`

**gallery\_images** `array\<object\>` *required*

The gallery images for this product, ordered by position.

**child attributes**

**content\_type** `string | null` *required*

Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.

Example: `image/jpeg`

**id** `string` *required*

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**global\_affiliate\_percentage** `number | null` *required*

Marketplace affiliate commission percentage for this product, or `null` if program is inactive.

Example: `6.9`

**global\_affiliate\_status** `GlobalAffiliateStatuses` *required*

The enrollment status of this product in the Whop marketplace global affiliate program.

Available options: `enabled`, `disabled`

**headline** `string | null` *required*

A short marketing headline displayed prominently on the product's product page.

Example: `A showroom finish that lasts three years`

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**member\_affiliate\_percentage** `number | null` *required*

Member referral commission percentage for this product, or `null` if program is inactive.

Example: `6.9`

**member\_affiliate\_status** `GlobalAffiliateStatuses` *required*

The enrollment status of this product in the member affiliate program.

Available options: `enabled`, `disabled`

**member\_count** `integer` *required*

Active memberships for this product. Returns `0` if the account has disabled public member counts.

Example: `42`

**metadata** `object | null` *required*

Custom key-value pairs stored on the product and included in payment and
membership webhook payloads. Max 50 keys, 100 characters per key, 500
characters per string value.

**owner\_user** `object` *required*

The user who owns the company that sells this product.

**child attributes**

**id** `string` *required*

The unique identifier for the user.

Example: `user_xxxxxxxxxxxxx`

**name** `string | null` *required*

The user's display name shown on their public profile.

Example: `Marcus Webb`

**username** `string` *required*

The user's unique username shown on their public profile.

Example: `marcuswebb`

**product\_tax\_code** `object | null` *required*

The tax classification code applied to purchases of this product for sales tax calculation. Null if no tax code is assigned.

**child attributes**

**id** `string` *required*

The unique identifier for the product tax code.

Example: `ptc_xxxxxxxxxxxxxx`

**name** `string` *required*

Human-readable name of this tax classification, such as 'Services'.

Example: `Services`

**product\_type** `ProductTaxCodeProductTypes` *required*

Broad product category this tax code covers, such as physical goods or digital services.

Available options: `physical`, `digital`, `services`

**published\_reviews\_count** `integer` *required*

The total number of published customer reviews for this product's company.

Example: `42`

**route** `string` *required*

URL slug in the product's public link, e.g. `ceramic-coating-package` in whop.com/company/ceramic-coating-package.

Example: `ceramic-coating-package`

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`

**updated\_at** `string\<date-time\>` *required*

The datetime the product was last updated.

Example: `2023-12-01T05:00:00.401Z`

**verified** `boolean` *required*

Whether this company has been verified by Whop's trust and safety team.

**visibility** `Visibility` *required*

Controls whether the product is visible to customers. When set to 'hidden', the product is only accessible via direct link.

Available options: `visible`, `hidden`, `archived`, `quick_link`


## Related topics

- [Products API](https://docs.whop.com/api-reference/products/product.md)
- [Product created](https://docs.whop.com/api-reference/legacy/products/product-created.md)
- [Product published](https://docs.whop.com/api-reference/legacy/products/product-published.md)
