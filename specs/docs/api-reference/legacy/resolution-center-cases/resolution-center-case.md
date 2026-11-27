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

# Resolution Center Case

> A resolution center case is a dispute or support case between a user and a company, tracking the issue, status, and outcome.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/resolution-center-cases/list-resolution-center-cases.md),
> the versioned API that new integrations should build on. This page stays fully
> supported.

```json Example theme={null}
{
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"customer_appealed": true,
	"customer_response_actions": ["respond"],
	"due_date": "2023-12-01T05:00:00.401Z",
	"id": "reso_xxxxxxxxxxxxx",
	"issue": "forgot_to_cancel",
	"member": {
		"id": "<string>"
	},
	"merchant_appealed": true,
	"merchant_response_actions": ["accept"],
	"payment": {
		"created_at": "2023-12-01T05:00:00.401Z",
		"currency": "usd",
		"id": "pay_xxxxxxxxxxxxxx",
		"paid_at": "2023-12-01T05:00:00.401Z",
		"subtotal": 6.9,
		"total": 6.9
	},
	"platform_response_actions": ["request_buyer_info"],
	"resolution_events": [
		{
			"action": "created",
			"created_at": "2023-12-01T05:00:00.401Z",
			"details": "I did not authorize this purchase.",
			"id": "revt_xxxxxxxxxxxxx",
			"reporter_type": "merchant"
		}
	],
	"status": "merchant_response_needed",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	}
}
```

**company** `object | null` *required*

The company involved in this resolution case. Null if the company no longer exists.

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**created\_at** `string\<date-time\>` *required*

The datetime the resolution was created.

Example: `2023-12-01T05:00:00.401Z`

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

**member** `object | null` *required*

The membership record associated with the disputed payment. Null if the membership no longer exists.

**child attributes**

**id** `string` *required*

The unique identifier for the extra public member.

**merchant\_appealed** `boolean` *required*

Whether the merchant has filed an appeal after the initial resolution
decision.

**merchant\_response\_actions** `array\<ResolutionCenterCaseMerchantResponses\>` *required*

The list of actions currently available to the merchant.

Available options: `accept`, `deny`, `request_more_info`, `appeal`, `respond`

**payment** `object` *required*

The payment record that is the subject of this resolution case.

**child attributes**

**created\_at** `string\<date-time\>` *required*

The datetime the payment was created.

Example: `2023-12-01T05:00:00.401Z`

**currency** `Currencies | null` *required*

The three-letter ISO currency code for this payment (e.g., 'usd', 'eur').

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**id** `string` *required*

The unique identifier for the payment.

Example: `pay_xxxxxxxxxxxxxx`

**paid\_at** `string\<date-time\> | null` *required*

The time at which this payment was successfully collected. Null if the payment has not yet succeeded. As a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**subtotal** `number | null` *required*

The payment amount before taxes and discounts are applied. In the currency specified by the currency field.

Example: `6.9`

**total** `number` *required*

The total amount charged to the customer for this payment, including taxes and after any discounts. In the currency specified by the currency field.

Example: `6.9`

**platform\_response\_actions** `array\<ResolutionCenterCasePlatformResponses\>` *required*

The list of actions currently available to the Whop platform for moderating this resolution.

Available options: `request_buyer_info`, `request_merchant_info`, `merchant_wins`, `merchant_refund`

**resolution\_events** `array\<object\>` *required*

The most recent 50 messages, actions, and status changes that have occurred during this resolution case.

**child attributes**

**action** `ResolutionCenterCaseActions` *required*

The type of action recorded in this event.

Available options: `created`, `responded`, `accepted`, `denied`, `appealed`, `withdrew`, `requested_more_info`, `escalated`, `dispute_opened`, `dispute_customer_won`, `dispute_merchant_won`

**created\_at** `string\<date-time\>` *required*

The datetime the resolution event was created.

Example: `2023-12-01T05:00:00.401Z`

**details** `string | null` *required*

The message body or additional context provided with this resolution event. Null if no details were included.

Example: `I did not authorize this purchase.`

**id** `string` *required*

The unique identifier for the resolution event.

Example: `revt_xxxxxxxxxxxxx`

**reporter\_type** `ResolutionCenterCaseReporters` *required*

The party who performed this action.

Available options: `merchant`, `customer`, `platform`, `system`

**status** `ResolutionCenterCaseStatuses` *required*

The current status of the resolution case, indicating which party needs to respond or if the case is closed.

Available options: `merchant_response_needed`, `customer_response_needed`, `merchant_info_needed`, `customer_info_needed`, `under_platform_review`, `customer_won`, `merchant_won`, `customer_withdrew`

**updated\_at** `string\<date-time\>` *required*

The datetime the resolution was last updated.

Example: `2023-12-01T05:00:00.401Z`

**user** `object` *required*

The customer (buyer) who filed this resolution case.

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


## Related topics

- [Whop CLI Commands](https://docs.whop.com/cli/commands.md)
- [Resolution Center Cases API](https://docs.whop.com/api-reference/resolution-center-cases/resolution-center-case.md)
- [List Resolution Center Cases](https://docs.whop.com/api-reference/resolution-center-cases/list-resolution-center-cases.md)
