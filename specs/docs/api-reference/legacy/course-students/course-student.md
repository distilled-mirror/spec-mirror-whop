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

# Course Student

> An enrollment record for a student in a course, including progress and completion metrics.

```json Example theme={null}
{
	"completed_lessons_count": 42,
	"completion_rate": 6.9,
	"course": {
		"experience": {
			"id": "exp_xxxxxxxxxxxxxx"
		},
		"id": "cors_xxxxxxxxxxxxx",
		"title": "Introduction to Ceramic Coating"
	},
	"first_interaction_at": "2023-12-01T05:00:00.401Z",
	"id": "<string>",
	"last_interaction_at": "2023-12-01T05:00:00.401Z",
	"total_lessons_count": 42,
	"user": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"profile_picture": {
			"url": "https://media.whop.com/abc123/optimized.jpg"
		},
		"username": "marcuswebb"
	}
}
```

**completed\_lessons\_count** `integer` *required*

The total number of lessons this student has marked as completed in the course.

Example: `42`

**completion\_rate** `number` *required*

The percentage of available lessons the student has completed, as a value from 0 to 100 rounded to two decimal places.

Example: `6.9`

**course** `object` *required*

The course this student is enrolled in.

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

**first\_interaction\_at** `string\<date-time\>` *required*

The timestamp when the student first interacted with this course, as a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the course student type.

**last\_interaction\_at** `string\<date-time\>` *required*

The timestamp when the student most recently interacted with this course, as a Unix timestamp.

Example: `2023-12-01T05:00:00.401Z`

**total\_lessons\_count** `integer` *required*

The total number of visible lessons available to this student in the course.

Example: `42`

**user** `object` *required*

The user profile of the enrolled student.

**child attributes**

**id** `string` *required*

The unique identifier for the user.

Example: `user_xxxxxxxxxxxxx`

**name** `string | null` *required*

The user's display name shown on their public profile.

Example: `Marcus Webb`

**profile\_picture** `object | null` *required*

The user's profile picture attachment with URL, content type, and file metadata. Null if using a legacy profile picture.

**child attributes**

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**username** `string` *required*

The user's unique username shown on their public profile.

Example: `marcuswebb`


## Related topics

- [Retrieve course student](https://docs.whop.com/api-reference/legacy/course-students/retrieve-course-student.md)
- [List course students](https://docs.whop.com/api-reference/legacy/course-students/list-course-students.md)
- [API Stability](https://docs.whop.com/api-reference/stability.md)
