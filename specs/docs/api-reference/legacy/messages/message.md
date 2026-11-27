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

# Message

> A message sent within an experience chat, direct message, or group chat.

```json Example theme={null}
{
	"content": "Hey, are you available for a **quick call**?",
	"created_at": "2023-12-01T05:00:00.401Z",
	"id": "<string>",
	"is_edited": true,
	"is_pinned": true,
	"mentions": ["<string>"],
	"mentions_everyone": true,
	"message_type": "regular",
	"poll": {
		"options": [
			{
				"id": "<string>",
				"text": "<string>"
			}
		]
	},
	"poll_votes": [
		{
			"count": 42,
			"option_id": "<string>"
		}
	],
	"reaction_counts": [
		{
			"count": 42,
			"emoji": "<string>"
		}
	],
	"replying_to_message_id": "<string>",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	},
	"view_count": 42
}
```

**content** `string | null` *required*

The message content formatted as Markdown. Null if the message has no text content.

Example: `Hey, are you available for a **quick call**?`

**created\_at** `string\<date-time\>` *required*

The timestamp when this message was originally created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

**is\_edited** `boolean` *required*

Whether the message content has been edited after it was originally sent.

**is\_pinned** `boolean` *required*

Whether this message is pinned to the top of the channel for easy access.

**mentions** `array\<string\>` *required*

A list of user IDs that are explicitly mentioned in this message.

**mentions\_everyone** `boolean` *required*

Whether the message includes an @everyone mention that notifies all channel
members.

**message\_type** `DmsPostTypes` *required*

The classification of this message: regular, system, or automated.

Available options: `regular`, `system`, `automated`

**poll** `object | null` *required*

A poll attached to this message. Null if the message does not contain a poll.

**child attributes**

**options** `array\<object\> | null` *required*

The options for the poll

**child attributes**

**id** `string` *required*

The unique identifier for the poll option.

**text** `string` *required*

The text of the poll option

**poll\_votes** `array\<object\>` *required*

Aggregated reaction counts on this message, filtered to a specific reaction type.

**child attributes**

**count** `integer` *required*

The number of users who reacted

Example: `42`

**option\_id** `string | null` *required*

The reaction that was used

**reaction\_counts** `array\<object\>` *required*

Aggregated reaction counts on this message, filtered to a specific reaction type.

**child attributes**

**count** `integer` *required*

The number of users who reacted

Example: `42`

**emoji** `string | null` *required*

The emoji that was used in shortcode format (:heart:)

**replying\_to\_message\_id** `string | null` *required*

The unique identifier of the message this post is replying to. Null if this is
not a reply.

**updated\_at** `string\<date-time\>` *required*

The timestamp when this message was last modified.

Example: `2023-12-01T05:00:00.401Z`

**user** `object` *required*

The user who authored this message.

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

**view\_count** `integer | null` *required*

The number of unique views this message has received. Null if view tracking is not enabled for this channel.

Example: `42`


## Related topics

- [Messages API](https://docs.whop.com/api-reference/messages/message.md)
- [Delete Message](https://docs.whop.com/api-reference/messages/delete-message.md)
- [List Messages](https://docs.whop.com/api-reference/messages/list-messages.md)
