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

# Reviews API

> A Review is a customer's star rating and optional written feedback on a product, shown on the product's store page.

Use the Reviews API to list a product's reviews, filtered by star rating or creation date, and retrieve a review by ID.

## Endpoints

| Endpoint | Request |
| - | - |
| [List reviews](https://docs.whop.com/api-reference/reviews/list-reviews.md) | GET `/reviews` |
| [Retrieve review](https://docs.whop.com/api-reference/reviews/retrieve-review.md) | GET `/reviews/{id}` |
| [Delete review](https://docs.whop.com/api-reference/reviews/delete-review.md) | DELETE `/reviews/{id}` |

## Attributes

**id** `string` *required*

The unique identifier for the review.

**account** `object` *required*

The account that this review was written for.

> [accordion] Properties
>
> **id** `string` *required*
>
> The unique identifier for the account.
>
> **route** `string` *required*
>
> URL slug for the account's store page. For example,
> `shine-time-auto-detailing` in whop.com/shine-time-auto-detailing.
>
> **title** `string` *required*
>
> The display name of the account shown to customers.

**attachments** `object[]` *required*

A list of files and media attached to the review.

> [accordion] Properties
>
> **id** `string` *required*
>
> The unique identifier for the attachment.
>
> **content\_type** `string | null` *required*
>
> Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.
>
> **filename** `string | null` *required*
>
> The original filename of the uploaded attachment, including its file
> extension.
>
> **url** `string | null` *required*
>
> A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

**created\_at** `string` *required*

When the review was created, as an ISO 8601 timestamp.

**description** `string | null` *required*

The body text of the review containing the user's detailed feedback. Returns
an empty string if no description was provided.

**joined\_at** `string | null` *required*

When the reviewer first joined the product, as an ISO 8601 timestamp. Null if
unknown.

**paid\_for\_product** `boolean | null` *required*

Whether the reviewer paid for the product. Null if the payment status is
unknown.

**product** `object` *required*

The product that this review was written for.

> [accordion] Properties
>
> **id** `string` *required*
>
> The unique identifier for the product.
>
> **title** `string` *required*
>
> The display name of the product shown to customers on the product page and in search results.

**published\_at** `string | null` *required*

When the review was published, as an ISO 8601 timestamp. Null if unknown.

**stars** `integer` *required*

The star rating given by the reviewer, from 1 to 5.

**status** `string` *required*

The moderation status of the review. Only published reviews are returned.

Available options: `published`

**title** `string | null` *required*

A short summary title for the review. Null if the reviewer did not provide
one.

**updated\_at** `string` *required*

When the review was last updated, as an ISO 8601 timestamp.

**user** `object` *required*

The user account of the person who wrote this review.

> [accordion] Properties
>
> **id** `string` *required*
>
> The unique identifier for the user.
>
> **name** `string | null` *required*
>
> The user's display name shown on their public profile.
>
> **username** `string` *required*
>
> The user's unique username shown on their public profile.

```json Review theme={null}
{
	"id": "rev_xxxxxxxxxxxxxx",
	"title": "Worth every penny",
	"description": "Two years on and the paint still beads like the day it was coated.",
	"stars": 5,
	"status": "published",
	"paid_for_product": true,
	"created_at": "2026-09-20T18:02:00.000Z",
	"updated_at": "2026-09-20T18:02:00.000Z",
	"published_at": "2026-09-20T18:02:00.000Z",
	"joined_at": "2024-08-11T14:00:00.000Z",
	"user": {
		"id": "user_xxxxxxxxxxxxxx",
		"name": "Dana Whitfield",
		"username": "danawhitfield"
	},
	"attachments": [
		{
			"id": "file_xxxxxxxxxxxxxx",
			"filename": "coated-hood.png",
			"content_type": "image/png",
			"url": "https://img.whop.com/uploads/coated-hood.png"
		}
	],
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing",
		"route": "shine-time-auto-detailing"
	},
	"product": {
		"id": "prod_xxxxxxxxxxxxxx",
		"title": "Ceramic Coating Package"
	}
}
```


## Related topics

- [Review Reports API](https://docs.whop.com/api-reference/review-reports/review-report.md)
- [Review](https://docs.whop.com/api-reference/legacy/reviews/review.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
