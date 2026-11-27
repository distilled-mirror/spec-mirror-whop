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

# Course Lesson Interaction

> A record of a user's progress on a specific lesson, tracking whether they have completed it.

```json Example theme={null}
{
	"completed": true,
	"course": {
		"experience": {
			"id": "exp_xxxxxxxxxxxxxx"
		},
		"id": "cors_xxxxxxxxxxxxx",
		"title": "Introduction to Ceramic Coating"
	},
	"created_at": "2023-12-01T05:00:00.401Z",
	"id": "crsli_xxxxxxxxxxxx",
	"lesson": {
		"chapter": {
			"id": "chap_xxxxxxxxxxxxx"
		},
		"id": "lesn_xxxxxxxxxxxxx",
		"title": "Preparing Paint for Ceramic Coating"
	},
	"user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"username": "marcuswebb"
	}
}
```

**completed** `boolean` *required*

Whether the user has finished this lesson.

**course** `object` *required*

The course that contains the tracked lesson.

**child attributes**

**experience** `object` *required*

The parent experience that this course belongs to.

**child attributes**

**id** `string` *required*

The unique identifier for the experience.

Example: `exp_xxxxxxxxxxxxxx`

**id** `string` *required*

The unique identifier for the course.

Example: `cors_xxxxxxxxxxxxx`

**title** `string | null` *required*

The display name of the course shown to students. Null if no title has been set.

Example: `Introduction to Ceramic Coating`

**created\_at** `string\<date-time\>` *required*

The datetime the lesson interaction was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the lesson interaction.

Example: `crsli_xxxxxxxxxxxx`

**lesson** `object` *required*

The lesson that this progress record belongs to.

**child attributes**

**chapter** `object` *required*

The parent chapter that contains this lesson.

**child attributes**

**id** `string` *required*

The unique identifier for the chapter.

Example: `chap_xxxxxxxxxxxxx`

**id** `string` *required*

The unique identifier for the lesson.

Example: `lesn_xxxxxxxxxxxxx`

**title** `string` *required*

The display name of the lesson shown to students. Maximum 120 characters.

Example: `Preparing Paint for Ceramic Coating`

**user** `object` *required*

The user whose progress is being tracked.

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


## Related topics

- [List course lesson interactions](https://docs.whop.com/api-reference/legacy/course-lesson-interactions/list-course-lesson-interactions.md)
- [Retrieve course lesson interaction](https://docs.whop.com/api-reference/legacy/course-lesson-interactions/retrieve-course-lesson-interaction.md)
- [Course lesson interaction completed](https://docs.whop.com/api-reference/legacy/course-lesson-interactions/course-lesson-interaction-completed.md)
