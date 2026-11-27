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

# Financing Applications API

> A Financing Application is an account's request to accept payment-financing methods, such as buy now, pay later.

Whop reviews the account before it can offer them. It is not a loan or business capital.

Use the Financing Applications API to [create](https://docs.whop.com/api-reference/financing-applications/create-financing-application.md) an application, [save answers](https://docs.whop.com/api-reference/financing-applications/update-financing-application.md) to its requirements, and [submit](https://docs.whop.com/api-reference/financing-applications/submit-financing-application.md) it for review. You can manage applications for your own account and its direct subaccounts.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Financing Applications](https://docs.whop.com/api-reference/financing-applications/list-financing-applications.md) | GET `/financing_applications` |
| [Retrieve Financing Application](https://docs.whop.com/api-reference/financing-applications/retrieve-financing-application.md) | GET `/financing_applications/{id}` |
| [Create Financing Application](https://docs.whop.com/api-reference/financing-applications/create-financing-application.md) | POST `/financing_applications` |
| [Submit Financing Application](https://docs.whop.com/api-reference/financing-applications/submit-financing-application.md) | POST `/financing_applications/{id}/submit` |
| [Update Financing Application](https://docs.whop.com/api-reference/financing-applications/update-financing-application.md) | PATCH `/financing_applications/{id}` |

## Attributes

**id** `string` *required*

The financing application ID, starting with inrq\_.

**account\_id** `string` *required*

The ID of the merchant account applying for payment financing, starting with
biz\_.

**created\_at** `string` *required*

When the application was created, as an ISO 8601 timestamp.

**feedback** `string | null`

Public review feedback when more information is requested or the application
is denied. Internal notes are never included.

**requirements** `object[]`

Merchant requirements and saved answers. Present on individual application responses, omitted from lists. Acceptance is collected separately on submit.

> [accordion] Properties
>
> **id** `string` *required*
>
> Requirement ID to use when updating this application's answers.
>
> **accepted\_file\_formats** `string[]` *required*
>
> Accepted document formats, such as pdf or jpeg.
>
> **field** `string` *required*
>
> The information requested, such as business\_description or
> processing\_statements.
>
> **file\_collection\_type** `string` *required*
>
> Whether the requirement accepts no file, one file, or multiple files.
>
> Available options: `none`, `single`, `multi`
>
> **files** `object[]` *required*
>
> Files attached to this requirement, including expiring download URLs.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > The file's ID, prefixed `file_`.
> >
> > **blurhash** `string | null` *required*
> >
> > BlurHash placeholder for an image or video. Null when unavailable or removed
> > by moderation.
> >
> > **content\_type** `string | null` *required*
> >
> > The original file's MIME type, for example `application/pdf`. It is kept when
> > moderation removes the file.
> >
> > **created\_at** `string` *required*
> >
> > When the file was created, as an ISO 8601 timestamp.
> >
> > **duration\_seconds** `number | null` *required*
> >
> > Audio or video duration in seconds. Null when unavailable or removed by
> > moderation.
> >
> > **filename** `string | null` *required*
> >
> > The original filename, including its extension.
> >
> > **height** `integer | null` *required*
> >
> > Image or video height in pixels. Null when unavailable or removed by
> > moderation.
> >
> > **multipart\_chunk\_size** `integer | null`
> >
> > The byte size each part (except the last) must be. Present only on create, and
> > only for multipart uploads.
> >
> > **multipart\_upload\_id** `string | null`
> >
> > The ID of the multipart upload, passed back to `complete`. Present only on
> > create, and only for multipart uploads.
> >
> > **multipart\_upload\_urls** `object[] | null`
> >
> > The presigned URL for each part. Present only on create, and only for multipart uploads.
> >
> > > [accordion] Properties
> > >
> > > **part\_number** `integer` *required*
> > >
> > > The 1-based index of this part within the multipart upload.
> > >
> > > **url** `string` *required*
> > >
> > > The presigned URL to PUT this part's bytes to.
> >
> > **object** `string` *required*
> >
> > The type of this object, always `file`.
> >
> > **optimized\_url** `string | null` *required*
> >
> > URL to the optimized file. `null` when no optimized version is available.
> >
> > **removed\_by\_moderation** `boolean` *required*
> >
> > Whether moderation removed the file. When `true`, `url` serves a replacement
> > image. `filename`, `size` and `content_type` still describe the original file.
> >
> > **size** `integer | null` *required*
> >
> > The file size in bytes. `null` until the upload has finished.
> >
> > **upload\_headers** `object`
> >
> > Headers to send with the upload PUT. Present only on create.
> >
> > **upload\_status** `string` *required*
> >
> > Where the file is in its upload lifecycle.
> >
> > Available options: `pending`, `processing`, `ready`, `failed`
> >
> > **upload\_url** `string | null`
> >
> > Presigned URL to PUT the file's bytes to. Present only on create, and only for
> > single-part uploads.
> >
> > **url** `string | null` *required*
> >
> > A URL to download the file. Public files get a permanent CDN URL, and private
> > files get a signed URL that expires. `null` until the upload has finished.
> > When moderation removes the file, a replacement image URL.
> >
> > **visibility** `string` *required*
> >
> > `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
> >
> > Available options: `public`, `private`
> >
> > **waveform\_url** `string | null` *required*
> >
> > Audio waveform URL. Null when unavailable or removed by moderation.
> >
> > **width** `integer | null` *required*
> >
> > Image or video width in pixels. Null when unavailable or removed by moderation.
>
> **max\_file\_size** `integer` *required*
>
> Maximum size in bytes for each uploaded document.
>
> **minimum\_length** `integer | null` *required*
>
> Minimum characters per text answer, when specified.
>
> **money** `object | null` *required*
>
> Saved maximum product price in its original currency. Null when unanswered or the saved legacy answer is invalid.
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
> **options** `string[]` *required*
>
> Allowed dropdown values. Empty when the answer is not an enumeration.
>
> **required** `boolean` *required*
>
> Whether this requirement must be completed before submission.
>
> **text\_collection\_type** `string` *required*
>
> Whether the requirement accepts no text, one value, or multiple values.
>
> Available options: `none`, `single`, `multi`
>
> **text\_format** `string` *required*
>
> Expected text format. max\_product\_price uses money instead of values.
>
> Available options: `plain`, `url`, `email`, `phone`, `number`, `checkbox`, `dropdown`
>
> **values** `string[]` *required*
>
> Saved text answers. Empty for money requirements. Read `money` for those.

**status** `string` *required*

The application's review state. Approval does not by itself indicate that financing payment methods are enabled on the account.

Available options: `requires_collection`, `awaiting_review`, `completed`, `approved`, `denied`

**terms** `object`

Current terms to present before submitting. Present on individual application responses.

> [accordion] Properties
>
> **content** `string` *required*
>
> The exact plain-text contract snapshot covered by this version. Present this
> content, the policies, and the disclosure before collecting acceptance.
>
> **disclosure** `string` *required*
>
> Affirmative acceptance statement to present to the merchant.
>
> **fees\_url** `string` *required*
>
> Additional financing fee information.
>
> **policies** `string[]` *required*
>
> Financing policies to present alongside the contract before collecting
> acceptance.
>
> **url** `string` *required*
>
> Source URL for the Splitit contract snapshot.
>
> **version** `string` *required*
>
> Content fingerprint to pass as merchant\_acceptance.terms\_version when submitting.

**updated\_at** `string` *required*

When the application was last updated, as an ISO 8601 timestamp.

```json FinancingApplication theme={null}
{
	"id": "inrq_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"status": "requires_collection",
	"feedback": null,
	"requirements": [
		{
			"id": "inrqi_yyyyyyyyyyyyyy",
			"field": "processing_statements",
			"required": true,
			"text_collection_type": "none",
			"file_collection_type": "multi",
			"text_format": "plain",
			"values": [],
			"money": null,
			"files": [
				{
					"id": "file_xxxxxxxxxxxxxx",
					"object": "file",
					"filename": "processing-statements-q3.pdf",
					"content_type": "application/pdf",
					"size": 482113,
					"url": "https://whop-assets-example.s3.amazonaws.com/uploads/processing-statements-q3.pdf",
					"optimized_url": null,
					"removed_by_moderation": false,
					"upload_status": "ready",
					"visibility": "private",
					"created_at": "2026-10-21T08:29:00.000Z",
					"width": null,
					"height": null,
					"blurhash": null,
					"duration_seconds": null,
					"waveform_url": null
				}
			],
			"minimum_length": null,
			"options": [],
			"accepted_file_formats": ["pdf", "png", "jpg"],
			"max_file_size": 20971520
		}
	],
	"terms": {
		"version": "2026-09-01",
		"url": "https://whop.com/splitit-terms/",
		"fees_url": "https://docs.whop.com/fees",
		"disclosure": "I have read and agree to the Splitit Retail Installment Contract terms.",
		"policies": [
			"All Resolution Center cases for financed transactions will be decided by Whop."
		],
		"content": "We are a retailer that offers installment payments through Splitit."
	},
	"created_at": "2026-10-20T12:00:00.000Z",
	"updated_at": "2026-10-21T08:30:00.000Z"
}
```


## Related topics

- [Create Financing Application](https://docs.whop.com/api-reference/financing-applications/create-financing-application.md)
- [Submit Financing Application](https://docs.whop.com/api-reference/financing-applications/submit-financing-application.md)
- [Update Financing Application](https://docs.whop.com/api-reference/financing-applications/update-financing-application.md)
