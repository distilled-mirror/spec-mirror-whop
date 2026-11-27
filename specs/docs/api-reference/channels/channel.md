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

# Channels API

> A Channel is a conversation on Whop that holds chat messages. A `private` channel is a one-to-one or group conversation.

A `community` channel is the chat in one of an account's Chat experiences. A `support` channel is a conversation between an account and one customer.

Use the Channels API to [create](https://docs.whop.com/api-reference/channels/create-channel.md) and [list](https://docs.whop.com/api-reference/channels/list-channels.md) channels, and to [retrieve](https://docs.whop.com/api-reference/channels/retrieve-channel.md) one by its own ID or its Chat experience's ID. You can also [accept](https://docs.whop.com/api-reference/channels/accept-dm-request.md) or [decline](https://docs.whop.com/api-reference/channels/decline-dm-request.md) direct-message requests, [mark a channel read](https://docs.whop.com/api-reference/channels/mark-channel-read.md), and [resolve](https://docs.whop.com/api-reference/channels/resolve-support-channel.md) or [reopen](https://docs.whop.com/api-reference/channels/reopen-support-channel.md) support conversations.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Channels](https://docs.whop.com/api-reference/channels/list-channels.md) | GET `/channels` |
| [Retrieve Channel](https://docs.whop.com/api-reference/channels/retrieve-channel.md) | GET `/channels/{id}` |
| [Create Channel](https://docs.whop.com/api-reference/channels/create-channel.md) | POST `/channels` |
| [Accept DM Request](https://docs.whop.com/api-reference/channels/accept-dm-request.md) | POST `/channels/{id}/accept` |
| [Decline DM Request](https://docs.whop.com/api-reference/channels/decline-dm-request.md) | POST `/channels/{id}/hide` |
| [Mark Channel Read](https://docs.whop.com/api-reference/channels/mark-channel-read.md) | POST `/channels/{id}/mark_read` |
| [Mark Channel Unread](https://docs.whop.com/api-reference/channels/mark-channel-unread.md) | POST `/channels/{id}/mark_unread` |
| [Pin Channel](https://docs.whop.com/api-reference/channels/pin-channel.md) | POST `/channels/{id}/pin` |
| [Reopen Support Channel](https://docs.whop.com/api-reference/channels/reopen-support-channel.md) | POST `/channels/{id}/reopen` |
| [Resolve Support Channel](https://docs.whop.com/api-reference/channels/resolve-support-channel.md) | POST `/channels/{id}/resolve` |
| [Unpin Channel](https://docs.whop.com/api-reference/channels/unpin-channel.md) | POST `/channels/{id}/unpin` |
| [Update Channel](https://docs.whop.com/api-reference/channels/update-channel.md) | PATCH `/channels/{id}` |
| [Delete Channel](https://docs.whop.com/api-reference/channels/delete-channel.md) | DELETE `/channels/{id}` |

## Attributes

**id** `string` *required*

Existing public channel ID, preserved across REST and chat clients.

**account\_id** `string | null` *required*

**created\_at** `string` *required*

Creation time as an ISO 8601 timestamp.

**deleted\_at** `string | null` *required*

When the channel was deleted or removed from your channel list, as an ISO 8601
timestamp. Null for a live channel.

**experience\_id** `string | null` *required*

**icon** `object | null` *required*

The channel icon. Community channels show the experience logo, falling back to the app icon. Support channels show the account logo. Group private channels show their own icon. Null for one-to-one private channels or when no image is set.

> [accordion] Properties
>
> **id** `string` *required*
>
> The file's ID, prefixed `file_`.
>
> **blurhash** `string | null` *required*
>
> BlurHash placeholder for an image or video. Null when unavailable or removed
> by moderation.
>
> **content\_type** `string | null` *required*
>
> The original file's MIME type, for example `application/pdf`. It is kept when
> moderation removes the file.
>
> **created\_at** `string` *required*
>
> When the file was created, as an ISO 8601 timestamp.
>
> **duration\_seconds** `number | null` *required*
>
> Audio or video duration in seconds. Null when unavailable or removed by
> moderation.
>
> **filename** `string | null` *required*
>
> The original filename, including its extension.
>
> **height** `integer | null` *required*
>
> Image or video height in pixels. Null when unavailable or removed by
> moderation.
>
> **multipart\_chunk\_size** `integer | null`
>
> The byte size each part (except the last) must be. Present only on create, and
> only for multipart uploads.
>
> **multipart\_upload\_id** `string | null`
>
> The ID of the multipart upload, passed back to `complete`. Present only on
> create, and only for multipart uploads.
>
> **multipart\_upload\_urls** `object[] | null`
>
> The presigned URL for each part. Present only on create, and only for multipart uploads.
>
> > [accordion] Properties
> >
> > **part\_number** `integer` *required*
> >
> > The 1-based index of this part within the multipart upload.
> >
> > **url** `string` *required*
> >
> > The presigned URL to PUT this part's bytes to.
>
> **object** `string` *required*
>
> The type of this object, always `file`.
>
> **optimized\_url** `string | null` *required*
>
> URL to the optimized file. `null` when no optimized version is available.
>
> **removed\_by\_moderation** `boolean` *required*
>
> Whether moderation removed the file. When `true`, `url` serves a replacement
> image. `filename`, `size` and `content_type` still describe the original file.
>
> **size** `integer | null` *required*
>
> The file size in bytes. `null` until the upload has finished.
>
> **upload\_headers** `object`
>
> Headers to send with the upload PUT. Present only on create.
>
> **upload\_status** `string` *required*
>
> Where the file is in its upload lifecycle.
>
> Available options: `pending`, `processing`, `ready`, `failed`
>
> **upload\_url** `string | null`
>
> Presigned URL to PUT the file's bytes to. Present only on create, and only for
> single-part uploads.
>
> **url** `string | null` *required*
>
> A URL to download the file. Public files get a permanent CDN URL, and private
> files get a signed URL that expires. `null` until the upload has finished.
> When moderation removes the file, a replacement image URL.
>
> **visibility** `string` *required*
>
> `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
>
> Available options: `public`, `private`
>
> **waveform\_url** `string | null` *required*
>
> Audio waveform URL. Null when unavailable or removed by moderation.
>
> **width** `integer | null` *required*
>
> Image or video width in pixels. Null when unavailable or removed by moderation.

**kind** `string` *required*

Available options: `private`, `community`, `support`

**last\_message\_at** `string | null` *required*

When the channel last recorded message activity, as an ISO 8601 timestamp.
Null when no message activity is recorded.

**last\_message\_id** `string | null` *required*

**membership** `object | null` *required*

> [accordion] Properties
>
> **id** `string | null` *required*
>
> Stored membership ID, or null for access-derived users.
>
> **channel\_id** `string` *required*
>
> **created\_at** `string | null` *required*
>
> When the stored channel membership was created, as an ISO 8601 timestamp. Null
> for users whose access does not have a stored membership.
>
> **deleted\_at** `string | null` *required*
>
> When the membership was removed, as an ISO 8601 timestamp. Null for current
> members and access-derived users.
>
> **inbox\_status** `string | null` *required*
>
> Your inbox state for a private channel. `null` for other users and other channel kinds.
>
> Available options: `requested`, `accepted`, `hidden`
>
> **last\_viewed\_at** `string | null` *required*
>
> Authorized participant read position. Community positions are visible only to
> the user themselves.
>
> **notification\_preference** `string | null` *required*
>
> Available options: `all`, `mentions`, `none`
>
> **pinned\_position** `integer | null` *required*
>
> **unread** `boolean | null` *required*
>
> Your unread state for the channel. `null` for other users.
>
> **updated\_at** `string | null` *required*
>
> When the stored channel membership last changed, as an ISO 8601 timestamp.
> Null for users whose access does not have a stored membership.
>
> **user** `object` *required*
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > User ID, prefixed `user_`.
> >
> > **badges** `object[]` *required*
> >
> > Public badges for the user, in display order. Empty when the user has no badges or is suspended.
> >
> > > [accordion] Properties
> > >
> > > **type** `string` *required*
> > >
> > > Badge category. `whop_team` identifies a Whop team member. `verified_partner` identifies a verified member of the Whop Partners program.
> > >
> > > Available options: `whop_team`, `verified_partner`
> >
> > **name** `string | null` *required*
> >
> > Display name.
> >
> > **profile\_picture** `object` *required*
> >
> > Avatar wrapper. Its `url` is always present, and is a generated placeholder when the user set no picture.
> >
> > > [accordion] Properties
> > >
> > > **url** `string` *required*
> > >
> > > Avatar image URL. Always present. It is a generated placeholder when the user set no picture.
> >
> > **username** `string` *required*
> >
> > Public username.
>
> **user\_id** `string` *required*
>
> User ID

**moderation** `object | null` *required*

> [accordion] Properties
>
> **ban\_media** `boolean` *required*
>
> Prevent members from posting media.
>
> **ban\_urls** `boolean` *required*
>
> Prevent members from posting links.
>
> **banned\_words** `string[] | null` *required*
>
> Words blocked in the channel. `null` when you cannot view this setting; an
> empty list means no words are blocked.
>
> **user\_posts\_cooldown\_seconds** `integer | null` *required*
>
> Seconds a member must wait between messages. `0` or `null` turns the cooldown
> off.
>
> **who\_can\_post** `string` *required*
>
> Who can post messages.
>
> Available options: `everyone`, `admins`
>
> **who\_can\_react** `string` *required*
>
> Who can react to messages.
>
> Available options: `everyone`, `no_one`

**name** `string | null` *required*

**notifications\_enabled** `boolean` *required*

Whether app notifications are on. Webhooks fire either way.

**support** `object | null` *required*

> [accordion] Properties
>
> **awaiting\_reply** `boolean` *required*
>
> **customer\_user\_id** `string` *required*
>
> **last\_message\_by** `string | null` *required*
>
> Available options: `customer`, `support`
>
> **resolved\_at** `string | null` *required*
>
> When the support conversation was resolved, as an ISO 8601 timestamp. Null
> while unresolved.
>
> **status** `string` *required*
>
> Available options: `unresolved`, `resolved`

**updated\_at** `string` *required*

Last channel change as an ISO 8601 timestamp.

```json Channel theme={null}
{
	"id": "feed_xxxxxxxxxxxxxxxxxxxxxx",
	"kind": "community",
	"name": "Detailing Talk",
	"icon": {
		"id": "file_xxxxxxxxxxxxxx",
		"object": "file",
		"filename": "detailing-talk.png",
		"content_type": "image/png",
		"size": 48213,
		"url": "https://img.whop.com/uploads/detailing-talk.png",
		"optimized_url": null,
		"removed_by_moderation": false,
		"upload_status": "ready",
		"visibility": "public",
		"created_at": "2026-09-02T16:20:11.000Z",
		"width": 512,
		"height": 512,
		"blurhash": "LEHV6nWB2yk8pyo0adR*.7kCMdnj",
		"duration_seconds": null,
		"waveform_url": null
	},
	"notifications_enabled": true,
	"account_id": "biz_xxxxxxxxxxxxxx",
	"experience_id": "exp_xxxxxxxxxxxxxx",
	"created_at": "2026-09-02T16:20:11.000Z",
	"updated_at": "2026-10-14T09:12:40.000Z",
	"deleted_at": null,
	"last_message_id": "post_xxxxxxxxxxxxxxxxxxxxxx",
	"last_message_at": "2026-10-14T09:12:40.000Z",
	"membership": null,
	"support": null,
	"moderation": {
		"ban_media": false,
		"ban_urls": true,
		"banned_words": ["giveaway", "crypto"],
		"who_can_post": "everyone",
		"who_can_react": "everyone",
		"user_posts_cooldown_seconds": 30
	}
}
```


## Related topics

- [Channel Members API](https://docs.whop.com/api-reference/channel-members/channel-member.md)
- [Delete Channel](https://docs.whop.com/api-reference/channels/delete-channel.md)
- [Update Channel](https://docs.whop.com/api-reference/channels/update-channel.md)
