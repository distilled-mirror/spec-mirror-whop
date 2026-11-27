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

# Channel Members API

> A Channel Member is a person in a channel.

A person added to a private or support channel has a stored membership, with a read position and a notification preference. A person who reads a channel through their access to its experience or support inbox has no stored membership. Their `id` is `null`, and their read position is not shown.

Use the Channel Members API to [list or search](https://docs.whop.com/api-reference/channel-members/list-channel-members.md) the people in a channel and [add](https://docs.whop.com/api-reference/channel-members/add-channel-member.md) someone to a private or support channel. You can also [change your notification preference](https://docs.whop.com/api-reference/channel-members/update-channel-member.md). [Remove](https://docs.whop.com/api-reference/channel-members/remove-channel-member.md) a member, or remove yourself to leave a channel.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Channel Members](https://docs.whop.com/api-reference/channel-members/list-channel-members.md) | GET `/channel_members` |
| [Retrieve Channel Member](https://docs.whop.com/api-reference/channel-members/retrieve-channel-member.md) | GET `/channel_members/{id}` |
| [Add Channel Member](https://docs.whop.com/api-reference/channel-members/add-channel-member.md) | POST `/channel_members` |
| [Update Channel Member](https://docs.whop.com/api-reference/channel-members/update-channel-member.md) | PATCH `/channel_members/{id}` |
| [Remove Channel Member](https://docs.whop.com/api-reference/channel-members/remove-channel-member.md) | DELETE `/channel_members/{id}` |

## Attributes

**id** `string | null` *required*

Stored membership ID, or null for access-derived users.

**channel\_id** `string` *required*

**created\_at** `string | null` *required*

When the stored channel membership was created, as an ISO 8601 timestamp. Null
for users whose access does not have a stored membership.

**deleted\_at** `string | null` *required*

When the membership was removed, as an ISO 8601 timestamp. Null for current
members and access-derived users.

**inbox\_status** `string | null` *required*

Your inbox state for a private channel. `null` for other users and other channel kinds.

Available options: `requested`, `accepted`, `hidden`

**last\_viewed\_at** `string | null` *required*

Authorized participant read position. Community positions are visible only to
the user themselves.

**notification\_preference** `string | null` *required*

Available options: `all`, `mentions`, `none`

**pinned\_position** `integer | null` *required*

**unread** `boolean | null` *required*

Your unread state for the channel. `null` for other users.

**updated\_at** `string | null` *required*

When the stored channel membership last changed, as an ISO 8601 timestamp.
Null for users whose access does not have a stored membership.

**user** `object` *required*

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

**user\_id** `string` *required*

User ID

```json ChannelMember theme={null}
{
	"id": "memb_1_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
	"channel_id": "feed_xxxxxxxxxxxxxxxxxxxxxx",
	"user_id": "user_xxxxxxxxxxxxxx",
	"user": {
		"id": "user_xxxxxxxxxxxxxx",
		"username": "danawhitfield",
		"name": "Dana Whitfield",
		"profile_picture": {
			"url": "https://img.whop.com/avatars/user_xxxxxxxxxxxxxx.png"
		},
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"created_at": "2026-09-02T16:20:11.000Z",
	"updated_at": "2026-10-14T09:15:02.000Z",
	"deleted_at": null,
	"last_viewed_at": "2026-10-14T09:15:02.000Z",
	"unread": false,
	"notification_preference": "mentions",
	"pinned_position": 0,
	"inbox_status": "accepted"
}
```


## Related topics

- [Remove Channel Member](https://docs.whop.com/api-reference/channel-members/remove-channel-member.md)
- [List Channel Members](https://docs.whop.com/api-reference/channel-members/list-channel-members.md)
- [Add Channel Member](https://docs.whop.com/api-reference/channel-members/add-channel-member.md)
