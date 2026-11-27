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

# Livestreams API

> A Livestream represents a live video broadcast in an experience.

After a recorded livestream ends, members can watch the replay, and moderators can pin the best recordings to feature them.

Use the Livestreams API to [list an experience's livestreams](https://docs.whop.com/api-reference/livestreams/list-livestreams.md), [retrieve one](https://docs.whop.com/api-reference/livestreams/retrieve-livestream.md) with its recording, and [pin](https://docs.whop.com/api-reference/livestreams/pin-livestream.md) or [unpin](https://docs.whop.com/api-reference/livestreams/unpin-livestream.md) recordings.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Livestreams](https://docs.whop.com/api-reference/livestreams/list-livestreams.md) | GET `/livestreams` |
| [Retrieve Livestream](https://docs.whop.com/api-reference/livestreams/retrieve-livestream.md) | GET `/livestreams/{id}` |
| [Retrieve Livestream Viewership](https://docs.whop.com/api-reference/livestreams/retrieve-livestream-viewership.md) | GET `/livestreams/{id}/viewership` |
| [Add Livestream Co-host](https://docs.whop.com/api-reference/livestreams/add-livestream-co-host.md) | POST `/livestreams/{id}/add_co_host` |
| [Pin Livestream](https://docs.whop.com/api-reference/livestreams/pin-livestream.md) | POST `/livestreams/{id}/pin` |
| [Remove Livestream Co-host](https://docs.whop.com/api-reference/livestreams/remove-livestream-co-host.md) | POST `/livestreams/{id}/remove_co_host` |
| [Unpin Livestream](https://docs.whop.com/api-reference/livestreams/unpin-livestream.md) | POST `/livestreams/{id}/unpin` |

## Attributes

**id** `string` *required*

Livestream ID, prefixed `lfeed_`.

**created\_at** `string` *required*

When the livestream was created, as an ISO 8601 timestamp.

**ended\_at** `string | null` *required*

When the livestream ended, as an ISO 8601 timestamp. `null` while it is live
or before it starts.

**pinned\_at** `string | null` *required*

When the recording was pinned or last moved, as an ISO 8601 timestamp. Pinned
livestreams are ordered by it, newest first. `null` when not pinned.

**recap** `object`

Top moments AI picked from chat, and chat activity. Only `retrieve` returns it, and only to callers with the `livestream:moderate` permission.

> [accordion] Properties
>
> **chat** `object | null` *required*
>
> Chat activity during the livestream. `null` while `status` is `pending`, before the livestream ends, or when the recap could not be started.
>
> > [accordion] Properties
> >
> > **chatter\_count** `integer` *required*
> >
> > How many different people sent those messages.
> >
> > **message\_count** `integer` *required*
> >
> > Chat messages sent from a minute before `recording_started_at`, or
> > `started_at` when the livestream was not recorded, to a minute after
> > `ended_at`.
> >
> > **per\_minute** `integer[]` *required*
> >
> > Chat messages sent in each minute from `recording_started_at`, or `started_at` when the livestream was not recorded, to `ended_at`. Messages sent outside that span are left out.
>
> **moments** `object[]` *required*
>
> Up to 5 top moments AI picked from what chat reacted to, earliest first. Empty while `status` is not `ready`, when nothing stood out, or when they could not be picked. A moment is left out once its `message` is removed from chat.
>
> > [accordion] Properties
> >
> > **description** `string` *required*
> >
> > One sentence on what happened and how chat reacted, in at most 140 characters.
> >
> > **end\_offset\_seconds** `integer` *required*
> >
> > Where chat's reaction to the moment ends, in seconds from the same point as
> > `offset_seconds`. Never before `offset_seconds`, and never after the next
> > moment's `offset_seconds`.
> >
> > **message** `object` *required*
> >
> > Chat message that best sums up the moment.
> >
> > > [accordion] Properties
> > >
> > > **id** `string` *required*
> > >
> > > ID of the chat message.
> > >
> > > **content** `string` *required*
> > >
> > > Message text, with whitespace collapsed and cut to at most 280 characters.
> > >
> > > **user\_name** `string | null` *required*
> > >
> > > Display name of the person who sent the message. `null` when their account no longer exists.
> >
> > **offset\_seconds** `integer` *required*
> >
> > Where the moment starts, shortly before chat reacted, in seconds from
> > `recording_started_at`, or from `started_at` when the livestream was not
> > recorded.
> >
> > **title** `string` *required*
> >
> > What happened on stream, in at most 60 characters.
>
> **status** `string` *required*
>
> `pending` while the recap is being generated and `ready` once it is. `unavailable` when the livestream has not ended, had no chat, or the recap could not be started. Retrieve the livestream again later while it is `pending`, or to retry a recap that could not be started.
>
> Available options: `pending`, `ready`, `unavailable`

**recording\_started\_at** `string | null` *required*

When the recording's first frame was captured, as an ISO 8601 timestamp.
Offsets into the recording count from here, not from `started_at`. `null`
until recording starts.

**recording\_status** `string | null` *required*

State of the recording. `null` when the livestream was not recorded.

Available options: `recording`, `processing`, `completed`, `failed`

**recording\_url** `string | null` *required*

A temporary link to the recording's video file. Retrieve the livestream again
for a fresh link. `null` until the recording is ready, or when it was deleted.

**started\_at** `string | null` *required*

When the livestream went live, as an ISO 8601 timestamp. `null` before it
starts.

**thumbnail\_url** `string | null` *required*

A temporary link to a still frame from the livestream. `null` when none was
captured.

**title** `string` *required*

Display title of the livestream. `Untitled Livestream` when the host did not
set one.

```json Livestream theme={null}
{
	"id": "lfeed_xxxxxxxxxxxxxxxxxxxxxx",
	"title": "Live detail: ceramic coating",
	"started_at": "2026-09-26T19:00:00.000Z",
	"ended_at": "2026-09-26T20:45:48.000Z",
	"recording_status": "completed",
	"recording_started_at": "2026-09-26T19:00:07.000Z",
	"pinned_at": "2026-09-27T09:00:00.000Z",
	"thumbnail_url": "https://img.whop.com/livestreams/lfeed_xxxxxxxxxxxxxxxxxxxxxx.jpg",
	"recording_url": "https://whop-livestreams-example.s3.amazonaws.com/room/lfeed_xxxxxxxxxxxxxxxxxxxxxx.mp4",
	"created_at": "2026-09-26T18:59:41.000Z"
}
```


## Related topics

- [Unpin Livestream](https://docs.whop.com/api-reference/livestreams/unpin-livestream.md)
- [Pin Livestream](https://docs.whop.com/api-reference/livestreams/pin-livestream.md)
- [Retrieve Livestream](https://docs.whop.com/api-reference/livestreams/retrieve-livestream.md)
