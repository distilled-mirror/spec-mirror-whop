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

# Media API

> A Media Asset is an AI-generated image or video created from a prompt and billed from an account balance.

When generation finishes, the asset includes a file that can be attached anywhere Whop accepts files.

Use the Media API to start a generation job and retrieve the asset while it processes or after it is ready.

## Endpoints

| Endpoint | Request |
| - | - |
| [Retrieve Media Asset](https://docs.whop.com/api-reference/media/retrieve-media-asset.md) | GET `/media/{id}` |
| [Generate Media Asset](https://docs.whop.com/api-reference/media/generate-media-asset.md) | POST `/media/generate` |

## Attributes

**id** `string` *required*

Media asset ID, prefixed `media_`.

**amount\_charged** `number | null` *required*

USD amount charged to the account's balance for this generation. `null` if the
generation wasn't billed.

**completed\_at** `string | null` *required*

ISO 8601 timestamp when the asset reached a terminal state. `null` while
`processing`.

**created\_at** `string` *required*

ISO 8601 timestamp when the generation was requested.

**currency** `string` *required*

Currency of `amount_charged`. Always `usd`.

**error\_message** `string | null` *required*

Why generation failed. `null` unless status is `failed`.

**file** `object | null` *required*

The produced file, usable anywhere attachments are accepted. `null` until the asset is `ready`.

> [accordion] Properties
>
> **id** `string` *required*
>
> File ID, prefixed `file_`.
>
> **url** `string` *required*
>
> CDN URL for downloading the file.

**generation** `object` *required*

The inputs the asset was generated from.

> [accordion] Properties
>
> **ad\_video** `object | null` *required*
>
> The video ad request. `null` unless the asset was generated with `ad_video`.
>
> > [accordion] Properties
> >
> > **ad\_id** `string | null` *required*
> >
> > The ad, prefixed `ad_`, whose landing page the video sells. `null` when the video sells the account's own products.
>
> **duration\_seconds** `number | null` *required*
>
> Requested video length in seconds. `null` for images.
>
> **prompt** `string` *required*
>
> What the asset was generated from. For a video ad, this is the prompt Whop
> wrote. It is empty until Whop writes it.
>
> **reference\_media** `string[]` *required*
>
> Reference file IDs (`file_` prefixed) used for the generation. Includes the
> video when the asset remade one, and the product photos Whop picked for a
> video ad.
>
> **resolution** `string | null` *required*
>
> Requested video resolution. `null` for images. `1080p` is not supported by Seedance 2.0 Fast or Mini; `4k` is only supported by Seedance 2.0.
>
> Available options: `480p`, `720p`, `1080p`, `4k`

**media\_type** `string` *required*

The kind of media this asset holds.

Available options: `video`, `image`

**source** `string` *required*

How the asset was created. Always `generated`.

Available options: `generated`

**status** `string` *required*

Lifecycle state: `processing` while generation runs, `ready` when the file is available, `failed` when generation failed and the charge was refunded.

Available options: `processing`, `ready`, `failed`

```json MediaAsset theme={null}
{
	"id": "media_xxxxxxxxxxx",
	"amount_charged": 0.25,
	"completed_at": "2026-06-01T12:01:30Z",
	"created_at": "2026-06-01T12:00:00Z",
	"currency": "usd",
	"error_message": null,
	"file": {
		"id": "file_xxxxxxxxxxxx",
		"url": "https://img.whop.com/file_xxxxxxxxxxxx.png"
	},
	"generation": {
		"ad_video": null,
		"duration_seconds": null,
		"prompt": "A clean product mockup on a white background",
		"reference_media": [],
		"resolution": null
	},
	"media_type": "image",
	"source": "generated",
	"status": "ready"
}
```


## Related topics

- [Files API](https://docs.whop.com/api-reference/files/file.md)
- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [Retrieve Media Asset](https://docs.whop.com/api-reference/media/retrieve-media-asset.md)
