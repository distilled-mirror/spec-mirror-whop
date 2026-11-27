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

# Forum Post

> A post or comment in a forum feed, supporting rich text, attachments, polls, and reactions.

```json Example theme={null}
{
	"attachments": [
		{
			"content_type": "image/jpeg",
			"filename": "document.pdf",
			"id": "<string>",
			"url": "https://media.whop.com/abc123/optimized.jpg"
		}
	],
	"comment_count": 42,
	"content": "## Ceramic Coating Care\n\nHere are the key steps...",
	"created_at": "2023-12-01T05:00:00.401Z",
	"id": "<string>",
	"is_edited": true,
	"is_pinned": true,
	"is_poster_admin": true,
	"like_count": 42,
	"parent_id": "<string>",
	"title": "Ceramic Coating Care - February 2025",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	},
	"view_count": 42
}
```

**attachments** `array\<object\>` *required*

All file attachments on this post, such as images, documents, and videos.

**child attributes**

**content\_type** `string | null` *required*

Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.

Example: `image/jpeg`

**filename** `string | null` *required*

The original filename of the uploaded attachment, including its file extension.

Example: `document.pdf`

**id** `string` *required*

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**comment\_count** `integer` *required*

The total number of direct comments on this post.

Example: `42`

**content** `string | null` *required*

The body of the forum post in Markdown format. Null if the post is paywalled and the current user does not have access.

Example: `## Ceramic Coating Care Here are the key steps...`

**created\_at** `string\<date-time\>` *required*

The time this post was created, as a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

**is\_edited** `boolean` *required*

Whether this post has been edited after its initial creation.

**is\_pinned** `boolean` *required*

Whether this post is pinned to the top of the forum feed.

**is\_poster\_admin** `boolean` *required*

Whether the author of this post is an admin of the company that owns the
forum.

**like\_count** `integer | null` *required*

The total number of like reactions this post has received.

Example: `42`

**parent\_id** `string | null` *required*

The unique identifier of the parent post. Null if this is a top-level post.

**title** `string | null` *required*

The headline of the forum post. Null if the post has no title.

Example: `Ceramic Coating Care - February 2025`

**updated\_at** `string\<date-time\>` *required*

The time this post was last updated, as a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**user** `object` *required*

The user who authored this forum post.

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

The total number of times this post has been viewed by users.

Example: `42`


## Related topics

- [Forums](https://docs.whop.com/developer/guides/forums.md)
- [List forum posts](https://docs.whop.com/api-reference/legacy/forum-posts/list-forum-posts.md)
- [Retrieve forum post](https://docs.whop.com/api-reference/legacy/forum-posts/retrieve-forum-post.md)
