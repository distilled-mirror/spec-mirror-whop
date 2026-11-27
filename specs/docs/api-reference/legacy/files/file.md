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

# File

> A file that has been uploaded or is pending upload.

```json Example theme={null}
{
	"content_type": "image/jpeg",
	"filename": "document.pdf",
	"id": "file_xxxxxxxxxxxxx",
	"size": "123.45",
	"upload_status": "pending",
	"url": "<string>",
	"visibility": "public"
}
```

**content\_type** `string | null` *required*

The MIME type of the uploaded file (e.g., image/jpeg, video/mp4, audio/mpeg).

Example: `image/jpeg`

**filename** `string | null` *required*

The original filename of the uploaded file, including its file extension.

Example: `document.pdf`

**id** `string` *required*

The unique identifier for the file.

Example: `file_xxxxxxxxxxxxx`

**size** `string | null` *required*

The file size in bytes. Null if the file has not finished uploading.

Example: `123.45`

**upload\_status** `UploadStatuses` *required*

The current upload status of the file (e.g., pending, ready).

Available options: `pending`, `processing`, `ready`, `failed`

**url** `string | null` *required*

The URL for accessing the file. For public files, this is a permanent CDN URL.
For private files, this is a signed URL that expires. Null if the file has not
finished uploading.

**visibility** `FileVisibility` *required*

Whether the file is publicly accessible or requires authentication.

Available options: `public`, `private`


## Related topics

- [Files API](https://docs.whop.com/api-reference/files/file.md)
- [Create File](https://docs.whop.com/api-reference/files/create-file.md)
- [List Files](https://docs.whop.com/api-reference/files/list-files.md)
