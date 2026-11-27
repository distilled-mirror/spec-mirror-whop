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

# Forum

> A discussion forum where members can create posts, comment, and react, belonging to an experience.

```json Example theme={null}
{
	"banned_words": ["<string>"],
	"banner_image": {
		"content_type": "image/jpeg",
		"filename": "document.pdf",
		"id": "<string>",
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"email_notification_preference": "all_admin_posts",
	"experience": {
		"id": "exp_xxxxxxxxxxxxxx",
		"is_public": true,
		"name": "Detailing Tips Chat"
	},
	"id": "<string>",
	"who_can_comment": "everyone",
	"who_can_post": "everyone"
}
```

**banned\_words** `array\<string\>` *required*

A list of words that are automatically filtered from posts in this forum.

**banner\_image** `object | null` *required*

The banner image displayed at the top of the forum page. Null if no banner has been uploaded.

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

**email\_notification\_preference** `ForumEmailNotificationPreferences` *required*

The email notification setting that controls which posts trigger email alerts. One of: all\_admin\_posts, only\_weekly\_summary, none.

Available options: `all_admin_posts`, `only_weekly_summary`, `none`

**experience** `object` *required*

The parent experience that this forum belongs to.

**child attributes**

**id** `string` *required*

The unique identifier for the experience.

Example: `exp_xxxxxxxxxxxxxx`

**is\_public** `boolean` *required*

Whether this experience is publicly visible to all users, including those
without a membership.

**name** `string` *required*

The display name of this experience shown to users in the product navigation. Maximum 255 characters.

Example: `Detailing Tips Chat`

**id** `string` *required*

The unique identifier for the entity

**who\_can\_comment** `ForumWhoCanCommentTypes` *required*

The permission level controlling who can comment on posts. One of: everyone, admins.

Available options: `everyone`, `admins`

**who\_can\_post** `ForumWhoCanPostTypes` *required*

The permission level controlling who can create new posts. One of: everyone, admins.

Available options: `everyone`, `admins`


## Related topics

- [Forums](https://docs.whop.com/developer/guides/forums.md)
- [Retrieve forum](https://docs.whop.com/api-reference/legacy/forums/retrieve-forum.md)
- [List forums](https://docs.whop.com/api-reference/legacy/forums/list-forums.md)
