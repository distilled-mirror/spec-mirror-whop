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

# Course

> A structured learning module containing chapters and lessons, belonging to an experience.

```json Example theme={null}
{
	"certificate_after_completion_enabled": true,
	"chapters": [
		{
			"id": "chap_xxxxxxxxxxxxx",
			"lessons": [
				{
					"id": "lesn_xxxxxxxxxxxxx",
					"lesson_type": "text",
					"order": 42,
					"thumbnail": {
						"url": "https://media.whop.com/abc123/optimized.jpg"
					},
					"title": "Preparing Paint for Ceramic Coating",
					"video_asset": {
						"duration_seconds": 42,
						"signed_playback_id": "<string>",
						"signed_thumbnail_playback_token": "<string>"
					}
				}
			],
			"order": 42,
			"title": "Getting Started"
		}
	],
	"chapters_count": 42,
	"completed_lessons_count": 42,
	"cover_image": "https://assets.whop.com/images/course-cover.jpg",
	"created_at": "2023-12-01T05:00:00.401Z",
	"description": "Learn paint correction and ceramic coating techniques from experienced detailers.",
	"experience": {
		"id": "exp_xxxxxxxxxxxxxx"
	},
	"id": "cors_xxxxxxxxxxxxx",
	"language": "en",
	"latest_lesson_created_at": "2023-12-01T05:00:00.401Z",
	"lesson_unlock_days": [42],
	"order": "123.45",
	"require_completing_lessons_in_order": true,
	"resume_lesson": {
		"id": "lesn_xxxxxxxxxxxxx"
	},
	"started_at": "2023-12-01T05:00:00.401Z",
	"tagline": "Learn to protect a showroom finish",
	"thumbnail": {
		"content_type": "image/jpeg",
		"filename": "document.pdf",
		"id": "<string>",
		"optimized_url": "https://media.whop.com/abc123/optimized.jpg",
		"source_url": "https://media.whop.com/abc123/original.jpg"
	},
	"title": "Introduction to Ceramic Coating",
	"total_duration_seconds": 42,
	"total_lessons_count": 42,
	"updated_at": "2023-12-01T05:00:00.401Z",
	"visibility": "visible"
}
```

**certificate\_after\_completion\_enabled** `boolean | null` *required*

Whether students receive a PDF certificate after completing all lessons in
this course. Null if the setting has not been configured.

**chapters** `array\<object\>` *required*

An ordered list of all chapters in this course, sorted by their display position.

**child attributes**

**id** `string` *required*

The unique identifier for the chapter.

Example: `chap_xxxxxxxxxxxxx`

**lessons** `array\<object\>` *required*

An ordered list of lessons in this chapter, sorted by display position. Hidden lessons are excluded for non-admin users.

**child attributes**

**id** `string` *required*

The unique identifier for the lesson.

Example: `lesn_xxxxxxxxxxxxx`

**lesson\_type** `LessonTypes` *required*

The content format of this lesson. One of: text, video, pdf, multi, quiz, knowledge\_check.

Available options: `text`, `video`, `pdf`, `multi`, `quiz`, `knowledge_check`

**order** `integer` *required*

The sort position of this lesson within its parent chapter, starting from zero.

Example: `42`

**thumbnail** `object | null` *required*

The thumbnail image displayed on lesson cards and previews. Null if no thumbnail has been uploaded.

**child attributes**

**url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**title** `string` *required*

The display name of the lesson shown to students. Maximum 120 characters.

Example: `Preparing Paint for Ceramic Coating`

**video\_asset** `object | null` *required*

The Mux video asset for video-type lessons, used for streaming playback. Null if this lesson has no hosted video.

**child attributes**

**duration\_seconds** `integer | null` *required*

The duration of the video in seconds

Example: `42`

**signed\_playback\_id** `string | null` *required*

The signed playback ID of the Mux asset

**signed\_thumbnail\_playback\_token** `string | null` *required*

The signed thumbnail playback token of the Mux asset

**order** `integer` *required*

The sort position of this chapter within its parent course, starting from zero.

Example: `42`

**title** `string` *required*

The display name of the chapter shown to students. Maximum 150 characters.

Example: `Getting Started`

**chapters\_count** `integer` *required*

The total number of chapters in this course, including chapters whose lessons are all hidden from the current user.

Example: `42`

**completed\_lessons\_count** `integer` *required*

The number of lessons in this course that the current user has marked as completed. Zero when the request is not made on behalf of a user.

Example: `42`

**cover\_image** `string | null` *required*

The URL of the course cover image shown on preview cards. Null if no cover image has been uploaded.

Example: `https://assets.whop.com/images/course-cover.jpg`

**created\_at** `string\<date-time\>` *required*

The datetime the course was created.

Example: `2023-12-01T05:00:00.401Z`

**description** `string | null` *required*

A brief summary of the course content and objectives. Null if no description has been set.

Example: `Learn paint correction and ceramic coating techniques fro…`

**experience** `object` *required*

The parent experience that this course belongs to.

**child attributes**

**id** `string` *required*

The unique identifier for the experience.

Example: `exp_xxxxxxxxxxxxxx`

**id** `string` *required*

The unique identifier for the course.

Example: `cors_xxxxxxxxxxxxx`

**language** `Languages` *required*

The spoken language of the video content, used to generate accurate closed captions. One of: en, es, it, pt, de, fr, pl, ru, nl, ca, tr, sv, uk, no, fi, sk, el, cs, hr, da, ro, bg.

Available options: `en`, `es`, `it`, `pt`, `de`, `fr`, `pl`, `ru`, `nl`, `ca`, `tr`, `sv`, `uk`, `no`, `fi`, `sk`, `el`, `cs`, `hr`, `da`, `ro`, `bg`

**latest\_lesson\_created\_at** `string\<date-time\> | null` *required*

The creation timestamp of the most recently added lesson visible to the current user. Null if the course has no lessons.

Example: `2023-12-01T05:00:00.401Z`

**lesson\_unlock\_days** `array\<integer\>` *required*

The distinct drip schedules, in days after the course start, of lessons
visible to the current user. Combine with startedAt to work out which have
unlocked. Empty when the user has not started the course or no lesson is on a
schedule.

**order** `string` *required*

The sort position of this course within its parent experience, as a decimal for flexible ordering.

Example: `123.45`

**require\_completing\_lessons\_in\_order** `boolean` *required*

Whether students must complete each lesson sequentially before advancing to
the next one.

**resume\_lesson** `object | null` *required*

The lesson the current user should continue from: their first incomplete lesson, or the first lesson when they have finished the course, have not started it, or can edit it. Null if the course has no lessons.

**child attributes**

**id** `string` *required*

The unique identifier for the lesson.

Example: `lesn_xxxxxxxxxxxxx`

**started\_at** `string\<date-time\> | null` *required*

The earliest time the current user is known to have started this course. Null if they have not started it. Drip unlock schedules are measured from this point.

Example: `2023-12-01T05:00:00.401Z`

**tagline** `string | null` *required*

A short marketing tagline displayed beneath the course title. Null if no tagline has been set.

Example: `Learn to protect a showroom finish`

**thumbnail** `object | null` *required*

The thumbnail image displayed on course cards and previews. Null if no thumbnail has been uploaded.

**child attributes**

**content\_type** `string | null` *required*

Uploaded file MIME type, such as image/jpeg, video/mp4, or audio/mpeg.

Example: `image/jpeg`

**filename** `string | null` *required*

The original filename of the uploaded attachment, including its file extension.

Example: `document.pdf`

**id** `string` *required*

**optimized\_url** `string | null` *required*

A pre-optimized URL for rendering this attachment on the client. This should be used for displaying attachments in apps.

Example: `https://media.whop.com/abc123/optimized.jpg`

**source\_url** `string | null` *required*

The original source URL of the attachment, such as a direct link to S3. This should never be displayed on the client and should always be passed through an Imgproxy transformer.

Example: `https://media.whop.com/abc123/original.jpg`

**title** `string | null` *required*

The display name of the course shown to students. Null if no title has been set.

Example: `Introduction to Ceramic Coating`

**total\_duration\_seconds** `integer` *required*

The combined duration in seconds of every hosted video across the lessons visible to the current user.

Example: `42`

**total\_lessons\_count** `integer` *required*

The number of lessons in this course visible to the current user.

Example: `42`

**updated\_at** `string\<date-time\>` *required*

The datetime the course was last updated.

Example: `2023-12-01T05:00:00.401Z`

**visibility** `CourseVisibilities` *required*

The visibility setting that controls whether this course appears to students. One of: visible, hidden.

Available options: `visible`, `hidden`


## Related topics

- [List courses](https://docs.whop.com/api-reference/legacy/courses/list-courses.md)
- [Retrieve course](https://docs.whop.com/api-reference/legacy/courses/retrieve-course.md)
- [Update course](https://docs.whop.com/api-reference/legacy/courses/update-course.md)
