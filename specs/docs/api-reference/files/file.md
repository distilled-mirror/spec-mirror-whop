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

# Files API

> A File is an uploaded document or media object, identified by a `file_` ID.

Creating a file returns a presigned destination; upload the bytes there and the file becomes `ready`.

Use the Files API to create a file, upload its content directly to storage (in one PUT, or in parts for large files), and retrieve it while polling for readiness. A ready file's ID can be attached wherever Whop accepts files.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Files](https://docs.whop.com/api-reference/files/list-files.md) | GET `/files` |
| [Retrieve File](https://docs.whop.com/api-reference/files/retrieve-file.md) | GET `/files/{id}` |
| [Create File](https://docs.whop.com/api-reference/files/create-file.md) | POST `/files` |
| [Complete File Multipart Upload](https://docs.whop.com/api-reference/files/complete-file-multipart-upload.md) | POST `/files/{id}/complete` |

## Attributes

**id** `string` *required*

The file's ID, prefixed `file_`.

**blurhash** `string | null` *required*

BlurHash placeholder for an image or video. Null when unavailable or removed
by moderation.

**content\_type** `string | null` *required*

The original file's MIME type, for example `application/pdf`. It is kept when
moderation removes the file.

**created\_at** `string` *required*

When the file was created, as an ISO 8601 timestamp.

**duration\_seconds** `number | null` *required*

Audio or video duration in seconds. Null when unavailable or removed by
moderation.

**filename** `string | null` *required*

The original filename, including its extension.

**height** `integer | null` *required*

Image or video height in pixels. Null when unavailable or removed by
moderation.

**multipart\_chunk\_size** `integer | null`

The byte size each part (except the last) must be. Present only on create, and
only for multipart uploads.

**multipart\_upload\_id** `string | null`

The ID of the multipart upload, passed back to `complete`. Present only on
create, and only for multipart uploads.

**multipart\_upload\_urls** `object[] | null`

The presigned URL for each part. Present only on create, and only for multipart uploads.

> [accordion] Properties
>
> **part\_number** `integer` *required*
>
> The 1-based index of this part within the multipart upload.
>
> **url** `string` *required*
>
> The presigned URL to PUT this part's bytes to.

**object** `string` *required*

The type of this object, always `file`.

**optimized\_url** `string | null` *required*

URL to the optimized file. `null` when no optimized version is available.

**removed\_by\_moderation** `boolean` *required*

Whether moderation removed the file. When `true`, `url` serves a replacement
image. `filename`, `size` and `content_type` still describe the original file.

**size** `integer | null` *required*

The file size in bytes. `null` until the upload has finished.

**upload\_headers** `object`

Headers to send with the upload PUT. Present only on create.

**upload\_status** `string` *required*

Where the file is in its upload lifecycle.

Available options: `pending`, `processing`, `ready`, `failed`

**upload\_url** `string | null`

Presigned URL to PUT the file's bytes to. Present only on create, and only for
single-part uploads.

**url** `string | null` *required*

A URL to download the file. Public files get a permanent CDN URL, and private
files get a signed URL that expires. `null` until the upload has finished.
When moderation removes the file, a replacement image URL.

**visibility** `string` *required*

`public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.

Available options: `public`, `private`

**waveform\_url** `string | null` *required*

Audio waveform URL. Null when unavailable or removed by moderation.

**width** `integer | null` *required*

Image or video width in pixels. Null when unavailable or removed by
moderation.

```json File theme={null}
{
	"id": "file_xxxxxxxxxxxxxx",
	"object": "file",
	"filename": "evidence.pdf",
	"content_type": "application/pdf",
	"size": 9670,
	"width": null,
	"height": null,
	"blurhash": null,
	"duration_seconds": null,
	"waveform_url": null,
	"visibility": "private",
	"upload_status": "ready",
	"url": "https://whop-assets-example.s3.amazonaws.com/uploads/2026-09-15/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/evidence.pdf",
	"optimized_url": null,
	"created_at": "2026-09-15T14:30:00.000Z",
	"removed_by_moderation": false
}
```


## Related topics

- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Chat API](https://docs.whop.com/developer/guides/chat.md)
- [Update Account](https://docs.whop.com/api-reference/accounts/update-account.md)
