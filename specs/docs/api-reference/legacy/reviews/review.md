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

# Review

> A user-submitted review of a company, including a star rating and optional text feedback.

```json Example theme={null}
{
	"attachments": [
		{
			"content_type": "image/jpeg",
			"filename": "document.pdf",
			"id": "<string>",
			"url": "https://media.whop.com/abc123/optimized.jpg"
		}
	],
	"company": {
		"id": "biz_xxxxxxxxxxxxxx",
		"route": "shine-time-auto-detailing",
		"title": "Shine Time Auto Detailing"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"description": "Two years on and the paint still beads like the day it was coated.",
	"id": "rev_xxxxxxxxxxxxxx",
	"joined_at": "2023-12-01T05:00:00.401Z",
	"paid_for_product": true,
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	},
	"published_at": "2023-12-01T05:00:00.401Z",
	"stars": 42,
	"status": "pending",
	"title": "A showroom finish that lasts",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	}
}
```

**attachments** `array\<object\>` *required*

A list of files and media attached to the review.

**child attributes**

**content\_type** `string | null` *required*

Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.

Example: `image/jpeg`

**filename** `string | null` *required*

The original filename of the uploaded attachment, including its file extension.

Example: `document.pdf`

**id** `string` *required*

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**company** `object` *required*

The company that this review was written for.

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

The datetime the review was created.

Example: `2023-12-01T05:00:00.401Z`

**description** `string | null` *required*

The body text of the review containing the user's detailed feedback. Returns an empty string if no description was provided.

Example: `Two years on and the paint still beads like the day it wa…`

**id** `string` *required*

The unique identifier for the review.

Example: `rev_xxxxxxxxxxxxxx`

**joined\_at** `string\<date-time\> | null` *required*

The timestamp of when the reviewer first joined the product. Null if unknown.

Example: `2023-12-01T05:00:00.401Z`

**paid\_for\_product** `boolean | null` *required*

Whether the reviewer paid for the product. Null if the payment status is
unknown.

**product** `object` *required*

The product that this review was written for.

**child attributes**

**id** `string` *required*

The unique identifier for the product.

Example: `prod_xxxxxxxxxxxxx`

**title** `string` *required*

The display name of the product shown to customers on the product page and in search results.

Example: `Ceramic Coating Package`

**published\_at** `string\<date-time\> | null` *required*

The timestamp of when the review was published. Null if the review has not been published yet.

Example: `2023-12-01T05:00:00.401Z`

**stars** `integer` *required*

The star rating given by the reviewer, from 1 to 5.

Example: `42`

**status** `ReviewStatus` *required*

The current moderation status of the review.

Available options: `pending`, `published`, `removed`

**title** `string | null` *required*

A short summary title for the review. Null if the reviewer did not provide one.

Example: `A showroom finish that lasts`

**updated\_at** `string\<date-time\>` *required*

The datetime the review was last updated.

Example: `2023-12-01T05:00:00.401Z`

**user** `object` *required*

The user account of the person who wrote this review.

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

- [Reviews API](https://docs.whop.com/api-reference/reviews/review.md)
- [Delete Review](https://docs.whop.com/api-reference/reviews/delete-review.md)
- [List Reviews](https://docs.whop.com/api-reference/reviews/list-reviews.md)
