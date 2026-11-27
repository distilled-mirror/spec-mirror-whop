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

# Course Lesson

> An individual learning unit within a chapter, which can contain text, video, PDF, or assessment content.

```json Example theme={null}
{
	"assessment_questions": [
		{
			"correct_answer": "<string>",
			"created_at": "2023-12-01T05:00:00.401Z",
			"id": "crsaq_xxxxxxxxxxxx",
			"image": {
				"content_type": "image/jpeg",
				"filename": "document.pdf",
				"id": "<string>",
				"url": "https://media.whop.com/abc123/optimized.jpg"
			},
			"options": [
				{
					"id": "crsaqo_xxxxxxxxxxx",
					"is_correct": true,
					"option_text": "<string>",
					"order": 42
				}
			],
			"order": 42,
			"question_text": "<string>",
			"question_type": "short_answer"
		}
	],
	"attachments": [
		{
			"content_type": "image/jpeg",
			"filename": "document.pdf",
			"id": "<string>",
			"url": "https://media.whop.com/abc123/optimized.jpg"
		}
	],
	"content": "In this lesson, we will cover washing, decontamination, and paint preparation before ceramic coating.",
	"created_at": "2023-12-01T05:00:00.401Z",
	"days_from_course_start_until_unlock": 42,
	"embed_id": "dQw4w9WgXcQ",
	"embed_type": "youtube",
	"id": "lesn_xxxxxxxxxxxxx",
	"lesson_type": "text",
	"main_pdf": {
		"content_type": "image/jpeg",
		"filename": "document.pdf",
		"id": "<string>",
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"order": 42,
	"thumbnail": {
		"url": "https://media.whop.com/abc123/optimized.jpg"
	},
	"title": "Preparing Paint for Ceramic Coating",
	"video_asset": {
		"asset_id": "<string>",
		"audio_only": true,
		"created_at": "2023-12-01T05:00:00.401Z",
		"duration_seconds": 42,
		"finished_uploading_at": "2023-12-01T05:00:00.401Z",
		"id": "mux_xxxxxxxxxxxxxx",
		"playback_id": "<string>",
		"signed_playback_id": "<string>",
		"signed_storyboard_playback_token": "<string>",
		"signed_thumbnail_playback_token": "<string>",
		"signed_video_playback_token": "<string>",
		"status": "uploading",
		"updated_at": "2023-12-01T05:00:00.401Z"
	},
	"visibility": "visible"
}
```

**assessment\_questions** `array\<object\>` *required*

The list of questions for quiz or knowledge check lessons. Empty for non-assessment lesson types.

**child attributes**

**correct\_answer** `string | null` *required*

The correct answer for the question. Used for short answer questions. Only
visible to admins (users with courses:update permission)

**created\_at** `string\<date-time\>` *required*

The datetime the assessment question was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the assessment question.

Example: `crsaq_xxxxxxxxxxxx`

**image** `object | null` *required*

Optional image attachment for the question

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

**options** `array\<object\>` *required*

The answer options for multiple choice/select questions

**child attributes**

**id** `string` *required*

The unique identifier for the assessment question option.

Example: `crsaqo_xxxxxxxxxxx`

**is\_correct** `boolean | null` *required*

Whether this option is a correct answer. Only visible to admins (users with
courses:update permission)

**option\_text** `string` *required*

The text of the answer option

**order** `integer` *required*

The order of this option within the question

Example: `42`

**order** `integer` *required*

The order of the question within its lesson

Example: `42`

**question\_text** `string` *required*

The text of the question

**question\_type** `CoursesAssessmentQuestionTypes` *required*

The type of the question

Available options: `short_answer`, `true_false`, `multiple_choice`, `multiple_select`

**attachments** `array\<object\>` *required*

All supplementary files attached to this lesson returned as a flat array rather than a paginated connection.

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

**content** `string | null` *required*

The Markdown content body of the lesson. Null if the lesson has no text content.

Example: `In this lesson, we will cover washing, decontamination, a…`

**created\_at** `string\<date-time\>` *required*

The datetime the lesson was created.

Example: `2023-12-01T05:00:00.401Z`

**days\_from\_course\_start\_until\_unlock** `integer | null` *required*

The number of days after a student starts the course before this lesson becomes accessible. Null if the lesson is available immediately.

Example: `42`

**embed\_id** `string | null` *required*

The external video identifier for embedded video lessons, such as a YouTube video ID or Loom share ID. Null if the lesson has no embed.

Example: `dQw4w9WgXcQ`

**embed\_type** `EmbedTypes | null` *required*

The platform type for the embedded video. One of: youtube, loom. Null if the lesson has no embed.

Available options: `youtube`, `loom`

**id** `string` *required*

The unique identifier for the lesson.

Example: `lesn_xxxxxxxxxxxxx`

**lesson\_type** `LessonTypes` *required*

The content format of this lesson. One of: text, video, pdf, multi, quiz, knowledge\_check.

Available options: `text`, `video`, `pdf`, `multi`, `quiz`, `knowledge_check`

**main\_pdf** `object | null` *required*

The primary PDF document for PDF-type lessons. Null if this lesson is not a PDF lesson or no PDF has been uploaded.

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

**asset\_id** `string | null` *required*

The Mux-provided ID of the asset

**audio\_only** `boolean` *required*

Whether this asset contains only audio

**created\_at** `string\<date-time\>` *required*

The datetime the mux asset was created.

Example: `2023-12-01T05:00:00.401Z`

**duration\_seconds** `integer | null` *required*

The duration of the video in seconds

Example: `42`

**finished\_uploading\_at** `string\<date-time\> | null` *required*

The time at which the video finished uploading

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The unique identifier for the mux asset.

Example: `mux_xxxxxxxxxxxxxx`

**playback\_id** `string | null` *required*

The public playback ID of the Mux asset

**signed\_playback\_id** `string | null` *required*

The signed playback ID of the Mux asset

**signed\_storyboard\_playback\_token** `string | null` *required*

The signed storyboard playback token of the Mux asset

**signed\_thumbnail\_playback\_token** `string | null` *required*

The signed thumbnail playback token of the Mux asset

**signed\_video\_playback\_token** `string | null` *required*

The signed video playback token of the Mux asset

**status** `MuxAssetStatuses` *required*

The status of the Mux asset

Available options: `uploading`, `created`, `ready`

**updated\_at** `string\<date-time\>` *required*

The datetime the mux asset was last updated.

Example: `2023-12-01T05:00:00.401Z`

**visibility** `LessonVisibilities` *required*

The visibility setting that controls whether this lesson appears to students. One of: visible, hidden.

Available options: `visible`, `hidden`


## Related topics

- [Create course lesson](https://docs.whop.com/api-reference/legacy/course-lessons/create-course-lesson.md)
- [Retrieve course lesson](https://docs.whop.com/api-reference/legacy/course-lessons/retrieve-course-lesson.md)
- [List course lessons](https://docs.whop.com/api-reference/legacy/course-lessons/list-course-lessons.md)
