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

# Membership

> A membership represents an active relationship between a user and a product. It tracks the user's access, billing status, and renewal schedule.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/memberships/membership.md), the versioned API that new
> integrations should build on. This page stays fully supported.

```json Example theme={null}
{
	"cancel_at_period_end": true,
	"cancel_option": "too_expensive",
	"cancelation_status": "won_back",
	"canceled_at": "2023-12-01T05:00:00.401Z",
	"cancellation_reason": "I found a better alternative.",
	"checkout_configuration_id": "ch_xxxxxxxxxxxxxxx",
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"currency": "usd",
	"custom_field_responses": [
		{
			"answer": "<string>",
			"id": "cfrp_xxxxxxxxxxxxx",
			"question": "<string>"
		}
	],
	"external_accounts": [
		{
			"external_id": "<string>",
			"platform": "<string>",
			"username": "<string>"
		}
	],
	"feedback_option": {
		"label": "<string>"
	},
	"formatted_renewal_price": "$25.00 / month",
	"id": "mem_xxxxxxxxxxxxxx",
	"initial_price_paid": "$25.00",
	"joined_at": "2023-12-01T05:00:00.401Z",
	"license_key": "A1B2C3-D4E5F6-G7H8I9",
	"manage_url": "https://whop.com/billing/manage/mem_abc123",
	"member": {
		"id": "mber_xxxxxxxxxxxxx"
	},
	"metadata": {},
	"payment_collection_paused": true,
	"plan": {
		"id": "plan_xxxxxxxxxxxxx",
		"metadata": {}
	},
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"metadata": {},
		"title": "Ceramic Coating Package"
	},
	"promo_code": {
		"id": "promo_xxxxxxxxxxxx"
	},
	"renewal_period_end": "2023-12-01T05:00:00.401Z",
	"renewal_period_start": "2023-12-01T05:00:00.401Z",
	"status": "trialing",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"user": {
		"email": "john.doe@example.com",
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"profile_pic": "<string>",
		"username": "marcuswebb"
	}
}
```

**cancel\_at\_period\_end** `boolean` *required*

Whether this membership is set to cancel at the end of the current billing
cycle. Only applies to memberships with a recurring plan.

**cancel\_option** `CancelOptions | null` *required*

The category selected for why the member canceled (e.g. too\_expensive, switching, missing\_features).

Available options: `too_expensive`, `switching`, `missing_features`, `technical_issues`, `bad_experience`, `unused`, `other`, `testing`

**cancelation\_status** `CancelationStatus | null` *required*

Whether the customer is canceling, left, or was won back. Null if the membership has no cancellation reason or its cancellation state is indeterminate.

Available options: `won_back`, `left`, `canceling`

**canceled\_at** `string\<date-time\> | null` *required*

The time the customer initiated cancellation of this membership. As a Unix timestamp. Null if the membership has not been canceled.

Example: `2023-12-01T05:00:00.401Z`

**cancellation\_reason** `string | null` *required*

Free-text explanation provided by the customer when canceling. Null if the customer did not provide a reason.

Example: `I found a better alternative.`

**checkout\_configuration\_id** `string | null` *required*

The ID of the checkout session/configuration that produced this membership, if any. Use this to map memberships back to the checkout configuration that created them.

Example: `ch_xxxxxxxxxxxxxxx`

**company** `object` *required*

The company this membership belongs to.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**created\_at** `string\<date-time\>` *required*

The datetime the membership was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies | null` *required*

The three-letter ISO currency code for this membership's billing. Null if the membership is free.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**custom\_field\_responses** `array\<object\>` *required*

The customer's responses to custom checkout questions configured on the product at the time of purchase.

**child attributes**

**answer** `string` *required*

The response a user gave to the specific question or field.

**id** `string` *required*

The unique identifier for the custom field response.

Example: `cfrp_xxxxxxxxxxxxx`

**question** `string` *required*

The question asked by the custom field

**external\_accounts** `array\<object\> | null` *required*

The member's accounts on outside platforms, such as Discord and Telegram. Empty when the member has none linked. Null in webhooks.

**child attributes**

**external\_id** `string` *required*

The member's user ID on the platform, such as a Discord user ID. Prefixed with the platform (`discord_\<external_id>`), it is the `external_account` value List Memberships filters by.

**platform** `string` *required*

The platform the member's account is on: `discord` or `telegram`.

**username** `string | null` *required*

The member's username on the platform. Null when Whop doesn't know it, such as for a membership imported with only a Discord ID.

**feedback\_option** `object | null` *required*

The account's own cancel reason the member picked, as they saw it. `null` when the member picked a default category or gave no reason.

**child attributes**

**label** `string` *required*

The option's label when the member picked it. Renaming the option later does not change it.

**formatted\_renewal\_price** `string | null` *required*

The recurring renewal price for this membership, formatted with currency symbol and billing interval. Null if the membership is not recurring.

Example: `$25.00 / month`

**id** `string` *required*

The unique identifier for the membership.

Example: `mem_xxxxxxxxxxxxxx`

**initial\_price\_paid** `string` *required*

The amount the customer paid when first purchasing this membership, formatted with currency symbol.

Example: `$25.00`

**joined\_at** `string\<date-time\> | null` *required*

The time the user first joined the company associated with this membership. As a Unix timestamp. Null if the member record does not exist.

Example: `2023-12-01T05:00:00.401Z`

**license\_key** `string | null` *required*

The software license key associated with this membership. Only present if the product includes a Whop Software Licensing experience. Null otherwise.

Example: `A1B2C3-D4E5F6-G7H8I9`

**manage\_url** `string | null` *required*

The URL where the customer can view and manage this membership, including cancellation and plan changes. Null if no member record exists.

Example: `https://whop.com/billing/manage/mem_abc123`

**member** `object | null` *required*

The member record linking the user to the company for this membership. Null if the member record has not been created yet.

**child attributes**

**id** `string` *required*

The unique identifier for the member.

Example: `mber_xxxxxxxxxxxxx`

**metadata** `object | null` *required*

Custom key-value pairs for the membership (commonly used for software
licensing, e.g., HWID). Max 50 keys, 100 chars per key, 500 chars per string
value.

**payment\_collection\_paused** `boolean` *required*

Whether recurring payment collection for this membership is temporarily paused
by the company.

**plan** `object` *required*

The plan the customer purchased to create this membership.

**child attributes**

**id** `string` *required*

The unique identifier for the plan.

Example: `plan_xxxxxxxxxxxxx`

**metadata** `object | null` *required*

Custom key-value pairs stored on the plan. Included in webhook payloads for payment and membership events. Max 50 keys, 100 chars per key, 500 chars per string value. The reserved keys `custom_cta` and `custom_cta_url`, when set, override the product's checkout call to action for this plan.

**product** `object` *required*

The product this membership grants access to.

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**metadata** `object | null` *required*

Custom key-value pairs stored on the product and included in payment and
membership webhook payloads. Max 50 keys, 100 characters per key, 500
characters per string value.

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`

**promo\_code** `object | null` *required*

The promotional code currently applied to this membership's billing. Null if no promo code is active.

**child attributes**

**id** `string` *required*

The unique identifier for the promo code.

Example: `promo_xxxxxxxxxxxx`

**renewal\_period\_end** `string\<date-time\> | null` *required*

The end of the current billing period for this recurring membership. As a Unix timestamp. Null if the membership is not recurring.

Example: `2023-12-01T05:00:00.401Z`

**renewal\_period\_start** `string\<date-time\> | null` *required*

The start of the current billing period for this recurring membership. As a Unix timestamp. Null if the membership is not recurring.

Example: `2023-12-01T05:00:00.401Z`

**status** `MembershipStatus` *required*

The current lifecycle status of the membership (e.g., active, trialing, past\_due, canceled, expired, completed).

Available options: `trialing`, `active`, `past_due`, `completed`, `canceled`, `expired`, `unresolved`, `scheduled`, `drafted`, `canceling`

**updated\_at** `string\<date-time\>` *required*

The datetime the membership was last updated.

Example: `2023-12-01T05:00:00.401Z`

**user** `object | null` *required*

The user who owns this membership. Null if the user account has been deleted.

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

**profile\_pic** `string` *required*

The URL of the user's profile picture. Use profilePicture for the full
attachment object.

**username** `string` *required*

The user's unique username shown on their public profile.

Example: `marcuswebb`


## Related topics

- [Memberships](https://docs.whop.com/developer/guides/memberships.md)
- [Memberships API](https://docs.whop.com/api-reference/memberships/membership.md)
- [Transfer Membership](https://docs.whop.com/api-reference/memberships/transfer-membership.md)
