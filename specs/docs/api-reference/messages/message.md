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

# Messages API

> A Message is one post in a channel. It holds Markdown content and can carry attachments, a poll, or a reply to another message.

Use the Messages API to [send](https://docs.whop.com/api-reference/messages/send-message.md), [edit](https://docs.whop.com/api-reference/messages/update-message.md), [delete](https://docs.whop.com/api-reference/messages/delete-message.md) and [pin](https://docs.whop.com/api-reference/messages/pin-message.md) messages. [List a channel's messages](https://docs.whop.com/api-reference/messages/list-messages.md) to read its history and to sync edits and deletions. You can also [react](https://docs.whop.com/api-reference/messages/react-to-message.md) with an emoji, [list who reacted](https://docs.whop.com/api-reference/messages/list-message-reactions.md), and [vote](https://docs.whop.com/api-reference/messages/vote-on-message.md) once on a poll. Each message shows its reaction and vote counts and whether you reacted or voted, but not who else voted.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Messages](https://docs.whop.com/api-reference/messages/list-messages.md) | GET `/messages` |
| [Retrieve Message](https://docs.whop.com/api-reference/messages/retrieve-message.md) | GET `/messages/{id}` |
| [List Message Reactions](https://docs.whop.com/api-reference/messages/list-message-reactions.md) | GET `/messages/{id}/reactions` |
| [Send Message](https://docs.whop.com/api-reference/messages/send-message.md) | POST `/messages` |
| [Pin Message](https://docs.whop.com/api-reference/messages/pin-message.md) | POST `/messages/{id}/pin` |
| [React to Message](https://docs.whop.com/api-reference/messages/react-to-message.md) | POST `/messages/{id}/react` |
| [Unpin Message](https://docs.whop.com/api-reference/messages/unpin-message.md) | POST `/messages/{id}/unpin` |
| [Remove Reaction from Message](https://docs.whop.com/api-reference/messages/remove-reaction-from-message.md) | POST `/messages/{id}/unreact` |
| [Vote on Message](https://docs.whop.com/api-reference/messages/vote-on-message.md) | POST `/messages/{id}/vote` |
| [Update Message](https://docs.whop.com/api-reference/messages/update-message.md) | PATCH `/messages/{id}` |
| [Delete Message](https://docs.whop.com/api-reference/messages/delete-message.md) | DELETE `/messages/{id}` |

## Attributes

**id** `string` *required*

Message ID, prefixed `post_`. Matches the `id` sent on create, when one was
sent.

**attachments** `object[]` *required*

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

**author** `object | null` *required*

> [accordion] Properties
>
> **id** `string` *required*
>
> User ID, prefixed `user_`.
>
> **badges** `object[]` *required*
>
> Public badges for the user, in display order. Empty when the user has no badges or is suspended.
>
> > [accordion] Properties
> >
> > **type** `string` *required*
> >
> > Badge category. `whop_team` identifies a Whop team member. `verified_partner` identifies a verified member of the Whop Partners program.
> >
> > Available options: `whop_team`, `verified_partner`
>
> **name** `string | null` *required*
>
> Display name.
>
> **profile\_picture** `object` *required*
>
> Avatar wrapper. Its `url` is always present, and is a generated placeholder when the user set no picture.
>
> > [accordion] Properties
> >
> > **url** `string` *required*
> >
> > Avatar image URL. Always present. It is a generated placeholder when the user set no picture.
>
> **username** `string` *required*
>
> Public username.

**author\_is\_team\_member** `boolean` *required*

`true` if the author is a team member of the account that owns this community
channel. `false` for partners, private and support channels, and deleted
messages.

**author\_user\_id** `string | null` *required*

**automation** `object | null` *required*

The automation that sent this message. `null` for a message no automation sent, a deleted message, or a caller without `automation:read` on the automation's account.

> [accordion] Properties
>
> **id** `string` *required*
>
> Automation ID, prefixed `ato_`.
>
> **name** `string` *required*
>
> The automation's name.

**channel\_id** `string` *required*

**content** `string | null` *required*

Markdown content. `null` for a deleted message.

**created\_at** `string` *required*

Creation time as an ISO 8601 timestamp.

**custom\_author** `object | null` *required*

> [accordion] Properties
>
> **display\_name** `string | null` *required*
>
> Webhook display name, or null to use the user's name.
>
> **profile\_picture** `object | null` *required*
>
> Webhook profile image, or null to use the user's image.
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

**deleted\_at** `string | null` *required*

Deletion time, or null for a live message.

**edited** `boolean` *required*

**embeds** `object[]` *required*

> [accordion] Properties
>
> **color** `integer | null` *required*
>
> **description** `string | null` *required*
>
> **fields** `object[]` *required*
>
> > [accordion] Properties
> >
> > **inline** `boolean` *required*
> >
> > **name** `string` *required*
> >
> > **value** `string` *required*
>
> **footer** `object | null` *required*
>
> Footer shown at the bottom of the embed. Null when the embed has no footer.
>
> > [accordion] Properties
> >
> > **icon** `object | null` *required*
> >
> > Small image shown beside the footer text. Null when the footer has no icon.
> >
> > > [accordion] Properties
> > >
> > > **blurhash** `string | null` *required*
> > >
> > > **height** `integer | null` *required*
> > >
> > > **url** `string` *required*
> > >
> > > **width** `integer | null` *required*
> >
> > **text** `string` *required*
>
> **image** `object | null` *required*
>
> > [accordion] Properties
> >
> > **blurhash** `string | null` *required*
> >
> > **height** `integer | null` *required*
> >
> > **url** `string` *required*
> >
> > **width** `integer | null` *required*
>
> **thumbnail** `object | null` *required*
>
> > [accordion] Properties
> >
> > **blurhash** `string | null` *required*
> >
> > **height** `integer | null` *required*
> >
> > **url** `string` *required*
> >
> > **width** `integer | null` *required*
>
> **timestamp\_at** `string | null` *required*
>
> Time supplied by the embed author, as an ISO 8601 timestamp in UTC. Null when
> missing or invalid.
>
> **title** `string | null` *required*
>
> **url** `string | null` *required*

**external\_message** `email_message | null` *required*

The message outside chat that this message was sent or received as. `object` says which kind it is, such as `email_message`. For an email, the first file in `attachments` named `email.html` holds the email's HTML body. The email's own attachments are also in `attachments`. `null` for a message that exists only in chat, or a deleted message.

> [accordion] Properties
>
> **id** `string` *required*
>
> The email's ID, prefixed `emsg_`.
>
> **from\_address** `string | null` *required*
>
> The address the email was sent from.
>
> **object** `string` *required*
>
> Always `email_message`.
>
> **status** `string` *required*
>
> The delivery status for this email.
>
> Available options: `received`, `spam`, `virus`, `queued`, `sent`, `delivered`, `bounced`, `complained`, `failed`, `refused_unsubscribed`, `refused_rate_limited`, `refused_spam`, `delayed`
>
> **subject** `string | null` *required*
>
> The email's subject.
>
> **to\_address** `string | null` *required*
>
> The address of the recipient this message is for.

**gifs** `object[]` *required*

GIFs attached to the message. Empty for a deleted message.

> [accordion] Properties
>
> **height** `integer` *required*
>
> Height of `original_url`, in pixels.
>
> **original\_url** `string` *required*
>
> Full-size GIF. HTTPS on a Klipy media host.
>
> **preview\_url** `string` *required*
>
> Smaller version for previews and placeholders. HTTPS on a Klipy media host.
>
> **slug** `string` *required*
>
> Identifier for the GIF.
>
> **title** `string` *required*
>
> GIF title.
>
> **url** `string` *required*
>
> URL the message renders the GIF from. HTTPS on a Klipy media host.
>
> **width** `integer` *required*
>
> Width of `original_url`, in pixels.

**link\_previews** `object[]` *required*

> [accordion] Properties
>
> **description** `string | null` *required*
>
> Page description, or null while unavailable.
>
> **favicon\_url** `string | null` *required*
>
> Site icon URL, or null when unavailable.
>
> **footer** `object | null` *required*
>
> Additional preview information, or null when unavailable.
>
> > [accordion] Properties
> >
> > **description** `string | null` *required*
> >
> > **icon\_url** `string` *required*
> >
> > **title** `string` *required*
>
> **image\_url** `string | null` *required*
>
> Preview image URL, or null when unavailable.
>
> **processing** `boolean` *required*
>
> Whether the server is still generating this preview.
>
> **title** `string | null` *required*
>
> Page title, or null while unavailable.
>
> **url** `string` *required*
>
> URL being previewed.

**mentioned\_user\_ids** `string[]` *required*

**message\_type** `string` *required*

Available options: `regular`, `system`, `automated`

**pinned** `boolean` *required*

**plain\_content** `string | null` *required*

Unformatted message text. null when the text is absent or the message is
deleted

**poll** `object | null` *required*

> [accordion] Properties
>
> **options** `object[]` *required*
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Option identifier within the poll.
> >
> > **text** `string` *required*
> >
> > **vote\_count** `integer` *required*
> >
> > **voted** `boolean` *required*
> >
> > Whether you voted for this option.

**reaction\_counts** `object[]` *required*

> [accordion] Properties
>
> **kind** `string` *required*
>
> Available options: `like`, `view`, `emoji`
>
> **reacted** `boolean` *required*
>
> Whether you reacted with this value.
>
> **total\_score** `integer` *required*
>
> **user\_count** `integer` *required*
>
> **value** `string | null` *required*
>
> The Unicode emoji for an emoji reaction. For other reaction kinds, the reaction's value.

**replying\_to\_message** `object | null`

The replied-to message. Omitted unless `include_replying_to_message=true`.
Includes all message fields except `replying_to_message`. `null` when this
message is deleted or no same-channel target exists.

**replying\_to\_message\_id** `string | null` *required*

**replying\_to\_user\_id** `string | null` *required*

User ID of the replied-to message's author, prefixed `user_`. `null` without a
reply in this channel, or when either message is deleted.

**updated\_at** `string` *required*

Last message change as an ISO 8601 timestamp.

**view\_count** `integer | null` *required*

Experience view count. `null` for DM-backed channels.

```json ChatMessage theme={null}
{
	"id": "post_xxxxxxxxxxxxxxxxxxxxxx",
	"channel_id": "feed_xxxxxxxxxxxxxxxxxxxxxx",
	"author_user_id": "user_yyyyyyyyyyyyyy",
	"author": {
		"id": "user_yyyyyyyyyyyyyy",
		"username": "marcuswebb",
		"name": "Marcus Webb",
		"profile_picture": {
			"url": "https://img.whop.com/avatars/user_yyyyyyyyyyyyyy.png"
		},
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"author_is_team_member": true,
	"custom_author": null,
	"content": "Which coating should we test on the demo car this Saturday? **Vote below.** Booking page: https://shinetime.example/book",
	"plain_content": "Which coating should we test on the demo car this Saturday? Vote below. Booking page: https://shinetime.example/book",
	"message_type": "regular",
	"edited": false,
	"pinned": true,
	"replying_to_message_id": null,
	"replying_to_user_id": null,
	"attachments": [
		{
			"id": "file_yyyyyyyyyyyyyy",
			"object": "file",
			"filename": "demo-car.jpg",
			"content_type": "image/jpeg",
			"size": 284190,
			"url": "https://img.whop.com/uploads/demo-car.jpg",
			"optimized_url": null,
			"removed_by_moderation": false,
			"upload_status": "ready",
			"visibility": "public",
			"created_at": "2026-09-02T16:20:11.000Z",
			"width": 1600,
			"height": 1067,
			"blurhash": "LEHV6nWB2yk8pyo0adR*.7kCMdnj",
			"duration_seconds": null,
			"waveform_url": null
		}
	],
	"poll": {
		"options": [
			{
				"id": "1",
				"text": "Graphene",
				"vote_count": 14,
				"voted": true
			},
			{
				"id": "2",
				"text": "Ceramic",
				"vote_count": 9,
				"voted": false
			}
		]
	},
	"mentioned_user_ids": [],
	"link_previews": [
		{
			"url": "https://shinetime.example/book",
			"title": "Book a detail | Shine Time",
			"description": "Pick a bay and a time for your next detail.",
			"image_url": "https://shinetime.example/og.png",
			"favicon_url": "https://shinetime.example/favicon.ico",
			"processing": false,
			"footer": {
				"title": "shinetime.example",
				"description": null,
				"icon_url": "https://shinetime.example/favicon.ico"
			}
		}
	],
	"gifs": [
		{
			"slug": "car-wash-shine-xxxxxxxx",
			"title": "Car Wash Shine",
			"url": "https://media.giphy.com/media/xxxxxxxx/giphy.gif",
			"original_url": "https://media.giphy.com/media/xxxxxxxx/giphy.gif",
			"preview_url": "https://media.giphy.com/media/xxxxxxxx/200w.gif",
			"width": 480,
			"height": 270
		}
	],
	"embeds": [
		{
			"color": 3447003,
			"title": "Saturday demo day",
			"url": "https://shinetime.example/demo",
			"description": "Live coating demo at Bay 2, 10:00 AM.",
			"timestamp_at": "2026-10-18T15:00:00.000Z",
			"thumbnail": {
				"url": "https://img.whop.com/uploads/demo-car-thumb.jpg",
				"width": 320,
				"height": 213,
				"blurhash": "LEHV6nWB2yk8pyo0adR*.7kCMdnj"
			},
			"image": null,
			"footer": {
				"text": "Shine Time Auto Detailing",
				"icon": null
			},
			"fields": [
				{
					"name": "Location",
					"value": "Bay 2",
					"inline": true
				},
				{
					"name": "Time",
					"value": "10:00 AM",
					"inline": true
				}
			]
		}
	],
	"reaction_counts": [
		{
			"kind": "emoji",
			"value": "🔥",
			"user_count": 6,
			"total_score": 6,
			"reacted": false
		}
	],
	"view_count": 182,
	"external_message": null,
	"automation": null,
	"created_at": "2026-10-14T09:12:40.000Z",
	"updated_at": "2026-10-14T09:12:40.000Z",
	"deleted_at": null
}
```


## Related topics

- [Chat API](https://docs.whop.com/developer/guides/chat.md)
- [Message](https://docs.whop.com/api-reference/legacy/messages/message.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
