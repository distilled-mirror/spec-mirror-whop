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

# AI Chat

> An AI-powered chat conversation belonging to a user, with optional scheduled automation.

```json Example theme={null}
{
	"agent_identifier": "general",
	"blended_token_usage": "123.45",
	"created_at": "2023-12-01T05:00:00.401Z",
	"id": "aich_xxxxxxxxxxxxx",
	"last_message_at": "2023-12-01T05:00:00.401Z",
	"message_count": 42,
	"notification_preference": "all",
	"title": "Weekly Revenue Analysis",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"user": {
		"id": "user_xxxxxxxxxxxxx"
	}
}
```

**agent\_identifier** `AiChatAgentIdentifiers` *required*

The AI agent that handles this chat. Set when the chat is created and fixed for its lifetime.

Available options: `general`, `support`

**blended\_token\_usage** `string` *required*

The total number of tokens consumed across all messages in this conversation.

Example: `123.45`

**created\_at** `string\<date-time\>` *required*

The datetime the ai chat was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the ai chat.

Example: `aich_xxxxxxxxxxxxx`

**last\_message\_at** `string\<date-time\> | null` *required*

The timestamp of the most recent message in this conversation. Null if no messages have been sent yet.

Example: `2023-12-01T05:00:00.401Z`

**message\_count** `integer` *required*

The total number of messages exchanged in this conversation.

Example: `42`

**notification\_preference** `AiChatNotificationPreferences` *required*

The notification preference for this AI chat. `all` delivers AI chat notifications and badges, while `none` mutes them.

Available options: `all`, `none`

**title** `string | null` *required*

A short descriptive title for this AI chat conversation. Null if no title has been set.

Example: `Weekly Revenue Analysis`

**updated\_at** `string\<date-time\>` *required*

The datetime the ai chat was last updated.

Example: `2023-12-01T05:00:00.401Z`

**user** `object` *required*

The user who owns this AI chat conversation.

**child attributes**

**id** `string` *required*

The unique identifier for the user.

Example: `user_xxxxxxxxxxxxx`


## Related topics

- [Delete ai chat](https://docs.whop.com/api-reference/legacy/ai-chats/delete-ai-chat.md)
- [Create ai chat](https://docs.whop.com/api-reference/legacy/ai-chats/create-ai-chat.md)
- [Retrieve ai chat](https://docs.whop.com/api-reference/legacy/ai-chats/retrieve-ai-chat.md)
