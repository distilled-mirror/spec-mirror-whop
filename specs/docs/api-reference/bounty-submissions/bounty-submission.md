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

# Bounty Submissions API

> A Bounty Submission is one worker's attempt on a bounty.

It starts as an in-progress attempt, enters the review queue when proof is submitted, and ends approved (paid from the bounty's escrowed pool) or denied.

Use the Bounty Submissions API to submit proof of completed work to a bounty, list the submissions you authored, and review the submissions on your bounties — across every bounty or narrowed to one.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Bounty Submissions](https://docs.whop.com/api-reference/bounty-submissions/list-bounty-submissions.md) | GET `/bounty_submissions` |
| [Retrieve Bounty Submission](https://docs.whop.com/api-reference/bounty-submissions/retrieve-bounty-submission.md) | GET `/bounty_submissions/{id}` |
| [Create Bounty Submission](https://docs.whop.com/api-reference/bounty-submissions/create-bounty-submission.md) | POST `/bounty_submissions` |
| [Submit Bounty Submission](https://docs.whop.com/api-reference/bounty-submissions/submit-bounty-submission.md) | POST `/bounty_submissions/{id}/submit` |
| [Cancel Bounty Submission](https://docs.whop.com/api-reference/bounty-submissions/cancel-bounty-submission.md) | DELETE `/bounty_submissions/{id}` |

## Attributes

**id** `string` *required*

Submission ID, prefixed `btys_`.

**bounty\_id** `string` *required*

The bounty the work was submitted to, prefixed `bnty_`.

**capture\_clips** `object[] | null` *required*

The submission's capture clips in recording order, each carrying its own review `status` and temporary signed artifact URLs. The full deliverable when `deliverable_type` is `data_capture`, populated only on single-submission reads; `null` on list responses (use `captured_clip_count` and `captured_duration_seconds` for the summary) and for other deliverable types. An attempt still in progress lists every clip it has recorded, including failed ones; once submitted, only clips that passed validation are listed, since only those are evidence.

> [accordion] Properties
>
> **id** `string` *required*
>
> Capture clip ID, prefixed `bclip_`.
>
> **bounty\_submission\_id** `string` *required*
>
> The bounty submission (attempt) this clip belongs to, prefixed `btys_`.
>
> **created\_at** `string` *required*
>
> When the clip was created, as an ISO 8601 timestamp.
>
> **duration\_seconds** `integer | null` *required*
>
> Server-validated clip duration in whole seconds. `null` until validation
> completes.
>
> **failure\_code** `string | null` *required*
>
> Stable validation failure code. `null` unless `status` is `failed`.
>
> **failure\_message** `string | null` *required*
>
> Human-readable validation failure reason. `null` unless `status` is `failed`.
>
> **frames\_url** `string | null` *required*
>
> Temporary signed URL for the video frame timestamp log. Returned only on
> single-clip reads for an authorized viewer; `null` on list responses or until
> the artifact is attached.
>
> **imu\_url** `string | null` *required*
>
> Temporary signed URL for the IMU (accelerometer + gyroscope) log. Returned
> only on single-clip reads for an authorized viewer; `null` on list responses
> or until the artifact is attached.
>
> **manifest\_url** `string | null` *required*
>
> Temporary signed URL for the capture manifest. Returned only on single-clip
> reads for an authorized viewer; `null` on list responses or until the artifact
> is attached.
>
> **ready\_at** `string | null` *required*
>
> When server-side validation completed successfully, as an ISO 8601 timestamp.
> `null` until then.
>
> **sequence** `integer` *required*
>
> The clip's stable order within the attempt, starting at 1.
>
> **status** `string` *required*
>
> Recording and validation state. `recording` is still capturing; `verifying` is running server-side validation; `ready` passed validation and counts toward the verified-duration payout gate; `failed` did not validate.
>
> Available options: `recording`, `verifying`, `ready`, `failed`
>
> **updated\_at** `string` *required*
>
> When the clip was last updated, as an ISO 8601 timestamp.
>
> **video\_url** `string | null` *required*
>
> Temporary signed URL for the synchronized MP4 video. Returned only on single-clip reads for an authorized viewer; `null` on list responses or until the artifact is attached.

**capture\_filename** `string | null` *required*

The vendor filename stem `Country_City_Site_Station_Operator`, derived from
the capture metadata. `null` until every component is present.

**captured\_clip\_count** `integer` *required*

Number of verified capture clips accepted for this submission so far. `0` for
submissions whose deliverable doesn't accumulate clips.

**captured\_duration\_seconds** `integer` *required*

Total verified duration of accepted capture clips, in whole seconds. `0` for
submissions whose deliverable doesn't accumulate clips.

**city** `string | null` *required*

Capture metadata: city the footage was recorded in. `null` unless capture
metadata was provided.

**claimed\_at** `string | null` *required*

When the worker claimed the submission, as an ISO 8601 timestamp.

**content** `string | null` *required*

Written proof the worker submitted with their work.

**country** `string | null` *required*

Capture metadata: country the footage was recorded in. `null` unless capture
metadata was provided.

**created\_at** `string` *required*

When the submission was created, as an ISO 8601 timestamp.

**deliverable\_type** `string | null` *required*

How the work arrived when it came in through the API in one shot, informational only — read the work from `deliverable_urls`, `files`, and `capture_clips` directly. `null` for submissions whose proof is a livestream recording, including ones that attached links or files on submit.

Available options: `content_url`, `media`, `data_capture`

**deliverable\_urls** `string[] | null` *required*

Links submitted as work, followed by temporary download URLs for the uploaded
files. `null` when the submission carries neither.

**denial\_reason** `string | null` *required*

Why the submission was denied, when a presentable reason exists. Always `null`
unless `status` is `denied`.

**device** `string | null` *required*

Capture metadata: device the footage was recorded on. `null` unless capture
metadata was provided.

**files** `object[]` *required*

Files uploaded as part of the work, in upload order. Empty when the submission has none.

> [accordion] Properties
>
> **id** `string` *required*
>
> File ID, prefixed `file_`.
>
> **attachment\_type** `string | null` *required*
>
> Broad kind of file.
>
> Available options: `image`, `video`, `audio`, `other`
>
> **content\_type** `string | null` *required*
>
> MIME type of the file.
>
> **filename** `string | null` *required*
>
> Name the file was uploaded with.
>
> **url** `string | null` *required*
>
> Temporary download URL for the file.

**fov** `integer | null` *required*

Capture metadata: horizontal field of view in degrees. `null` when not
reported.

**latest\_proof\_livestream\_feed** `object | null` *required*

Latest public proof livestream attached to the submission.

> [accordion] Properties
>
> **id** `string` *required*
>
> Livestream feed ID.
>
> **ended\_at** `string | null` *required*
>
> When the proof livestream ended, as an ISO 8601 timestamp. `null` while it is
> still live — a feed with a `started_at` and no `ended_at` is streaming right
> now.
>
> **recording\_status** `string | null` *required*
>
> Recording lifecycle state.
>
> Available options: `recording`, `processing`, `completed`, `failed`
>
> **recording\_url** `string | null` *required*
>
> Playback URL for a completed proof recording, when available.
>
> **started\_at** `string | null` *required*
>
> When the proof livestream went live, as an ISO 8601 timestamp. `null` before
> it starts.
>
> **thumbnail\_url** `string | null` *required*
>
> Current proof thumbnail URL, when available.
>
> **title** `string` *required*
>
> Display title for the proof livestream.

**operator** `string | null` *required*

Capture metadata: identifier of the person who recorded the footage. `null`
unless capture metadata was provided.

**resolved\_at** `string | null` *required*

When the submission was approved or denied, as an ISO 8601 timestamp. `null`
until then.

**site** `string | null` *required*

Capture metadata: site or venue the footage was recorded at. `null` unless
capture metadata was provided.

**station** `string | null` *required*

Capture metadata: station or position within the site. `null` unless capture
metadata was provided.

**status** `string | null` *required*

Lifecycle state. `in_progress` submissions are active attempts that have not submitted proof yet; `submitted` submissions await review; `approved` submissions were accepted and paid; `denied` submissions were rejected. `null` when the attempt ended without proof, taking it out of the public lifecycle — those attempts are absent from every public list and read.

Available options: `in_progress`, `submitted`, `approved`, `denied`

**submitted\_at** `string | null` *required*

When proof was submitted for review, as an ISO 8601 timestamp. `null` while
the attempt is in progress.

**updated\_at** `string` *required*

When the submission was last updated, as an ISO 8601 timestamp.

**worker** `object` *required*

User who submitted the work.

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

```json BountySubmission theme={null}
{
	"id": "btys_xxxxxxxxxxxxxx",
	"bounty_id": "bnty_xxxxxxxxxxxxxx",
	"capture_clips": null,
	"capture_filename": null,
	"captured_clip_count": 0,
	"captured_duration_seconds": 0,
	"city": null,
	"claimed_at": "2026-09-10T09:00:00.000Z",
	"content": "Ceramic coating reveal, filmed at the Burnet Rd bay.",
	"country": null,
	"created_at": "2026-09-10T09:00:00.000Z",
	"deliverable_type": "content_url",
	"deliverable_urls": [
		"https://www.tiktok.com/@shinetime/video/7412345678901234567"
	],
	"denial_reason": null,
	"device": null,
	"files": [
		{
			"id": "file_xxxxxxxxxxxxxx",
			"attachment_type": "image",
			"content_type": "image/png",
			"filename": "ceramic-coating-reveal.png",
			"url": "https://whop-assets-example.s3.amazonaws.com/uploads/image/2026-09-12/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
		}
	],
	"fov": null,
	"latest_proof_livestream_feed": null,
	"operator": null,
	"resolved_at": "2026-09-13T11:20:00.000Z",
	"site": null,
	"station": null,
	"status": "approved",
	"submitted_at": "2026-09-12T18:45:00.000Z",
	"updated_at": "2026-09-13T11:20:00.000Z",
	"worker": {
		"id": "user_xxxxxxxxxxxxxx",
		"name": "Dana Whitfield",
		"profile_picture": {
			"url": "https://ui-avatars.com/api/"
		},
		"username": "danawhitfield",
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	}
}
```


## Related topics

- [Bounties API](https://docs.whop.com/api-reference/bounties/bounty.md)
- [List Bounty Submissions](https://docs.whop.com/api-reference/bounty-submissions/list-bounty-submissions.md)
- [Create Bounty Submission](https://docs.whop.com/api-reference/bounty-submissions/create-bounty-submission.md)
