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

# Bounties API

> A Bounty is a paid task posted by an account or user.

The reward is held in escrow when the bounty publishes, workers submit proof of completed work, and each accepted submission is paid out until every winner slot fills.

Use the Bounties API to create and publish a bounty, list an account's bounties for reporting or dashboards, list the bounties a user can work or has participated in, and retrieve a single bounty by ID.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Bounties](https://docs.whop.com/api-reference/bounties/list-bounties.md) | GET `/bounties` |
| [Retrieve Bounty](https://docs.whop.com/api-reference/bounties/retrieve-bounty.md) | GET `/bounties/{id}` |
| [List Public Submissions](https://docs.whop.com/api-reference/bounties/list-public-submissions.md) | GET `/bounties/{bounty_id}/submissions` |
| [Retrieve Public Submission](https://docs.whop.com/api-reference/bounties/retrieve-public-submission.md) | GET `/bounties/{bounty_id}/submissions/{id}` |
| [Create Bounty](https://docs.whop.com/api-reference/bounties/create-bounty.md) | POST `/bounties` |
| [Cancel](https://docs.whop.com/api-reference/bounties/cancel.md) | POST `/bounties/{id}/cancel` |
| [Update Bounty](https://docs.whop.com/api-reference/bounties/update-bounty.md) | PATCH `/bounties/{id}` |

## Attributes

**id** `string` *required*

Bounty ID, prefixed `bnty_`.

**accepted\_deliverable\_types** `string[]` *required*

The deliverable shapes this bounty accepts. Every bounty accepts any combination of `content_url` (posted links) and `media` (uploaded files), except `data_capture` bounties, whose proof is clips recorded in the Whop app that accumulate on the attempt.

Available options: `content_url`, `media`, `data_capture`

**accepted\_submissions\_count** `integer` *required*

Submissions accepted so far.

**accepted\_submissions\_limit** `integer` *required*

Number of submissions that can be accepted (winner slots).

**accepted\_submissions\_per\_user\_limit** `integer` *required*

How many winner slots one worker can win. Defaults to `1`. Wins plus proofs
awaiting review never exceed this number, and a worker runs one attempt at a
time. Cannot exceed `accepted_submissions_limit`.

**active\_proof\_livestream\_feeds** `object[]` *required*

Proof livestreams live on this bounty right now, newest first — workers streaming their attempts. Empty when nothing is live.

> [accordion] Properties
>
> **id** `string` *required*
>
> Livestream feed ID.
>
> **host** `object | null` *required*
>
> User hosting the proof livestream — the worker streaming their attempt. `null` if the host account no longer exists.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > User ID, prefixed `user_`.
> >
> > **badges** `object[]` *required*
> >
> > Public badges for the user, in display order. Empty when the user has no badges or is suspended.
> >
> > > [accordion] Properties
> > >
> > > **type** `string` *required*
> > >
> > > Badge category. `whop_team` identifies a Whop team member. `verified_partner` identifies a verified member of the Whop Partners program.
> > >
> > > Available options: `whop_team`, `verified_partner`
> >
> > **name** `string | null` *required*
> >
> > Display name.
> >
> > **profile\_picture** `object` *required*
> >
> > Avatar wrapper. Its `url` is always present, and is a generated placeholder when the user set no picture.
> >
> > > [accordion] Properties
> > >
> > > **url** `string` *required*
> > >
> > > Avatar image URL. Always present. It is a generated placeholder when the user set no picture.
> >
> > **username** `string` *required*
> >
> > Public username.
>
> **title** `string` *required*
>
> Display title for the proof livestream.

**affiliate\_share\_amount** `number` *required*

What a referrer earns per accepted submission when the worker arrived through
their affiliate link, in whole currency units, at the standard platform fee
rate. Taken out of the worker's post-fee reward rather than added on top. `0`
when the bounty pays no affiliate share, including bounties tied to no
account, which cannot record a referral.

**allowed\_country\_codes** `string[]` *required*

Countries whose residents can work the bounty, as ISO 3166 alpha-2 codes.
Empty means worldwide.

**awaiting\_review\_submissions\_count** `integer` *required*

Submissions delivered and waiting on review. A subset of
`unresolved_submissions_count`, which also counts attempts still in progress.

**budget\_amount** `number` *required*

Total gross budget committed to the bounty: `gross_reward_amount` times
`accepted_submissions_limit`.

**business\_goal\_type** `string | null` *required*

What the poster wants the work to achieve, declared once at create. `null` for bounties created before the taxonomy rolled out.

Available options: `clipping`, `post_engagement`, `owned_account_growth`, `ugc_content`, `local_activation`, `data_capture`, `other`

**cancel\_requested\_at** `string | null` *required*

When cancellation was requested, as an ISO 8601 timestamp. On a `closed`
bounty this means the cancel is pending: submissions are stopped and the
bounty cancels once in-flight submissions resolve. On a `canceled` bounty it
records when the cancellation was requested. `null` when no cancellation was
ever requested.

**capture\_spec** `object | null` *required*

The technical contract footage must be recorded against. Present only on `data_capture` bounties; `null` for every other goal type.

> [accordion] Properties
>
> **filename\_pattern** `string` *required*
>
> The naming convention for uploaded files, built from the required metadata
> fields.
>
> **imu** `object` *required*
>
> Inertial measurement unit (IMU) recording requirements.
>
> > [accordion] Properties
> >
> > **device\_motion\_units** `string` *required*
> >
> > Units for the device-motion channels, as a compact key=unit string.
> >
> > **magnetometer\_units** `string` *required*
> >
> > Units for the magnetometer channel.
> >
> > **min\_rate\_hz** `number` *required*
> >
> > Minimum sustained IMU sample rate in hertz for a clip to pass validation.
> >
> > **target\_rate\_hz** `integer` *required*
> >
> > Target IMU sample rate in hertz.
> >
> > **warmup\_min\_rate\_hz** `number` *required*
> >
> > Minimum IMU sample rate in hertz tolerated during the warmup window.
> >
> > **warmup\_ns** `integer` *required*
> >
> > Startup window, in nanoseconds, during which the relaxed warmup rate applies.
>
> **manifest\_schema\_version** `integer` *required*
>
> Schema version the client must stamp on the capture manifest it uploads.
>
> **min\_clip\_duration\_seconds** `integer` *required*
>
> Minimum length of a single clip, in seconds.
>
> **min\_total\_verified\_duration\_seconds** `integer` *required*
>
> Total verified footage a submission must accumulate across all its clips
> before it can be submitted, in seconds. Always a whole number of hours.
>
> **required\_metadata\_fields** `string[]` *required*
>
> Metadata fields a submission must provide, matching the `metadata` object on
> the submissions API.
>
> **single\_continuous\_take** `boolean` *required*
>
> Whether each clip must be one uninterrupted recording rather than stitched
> segments.
>
> **video** `object` *required*
>
> Video recording requirements.
>
> > [accordion] Properties
> >
> > **bitrate\_ceiling\_mbps** `integer` *required*
> >
> > Maximum acceptable average bitrate, in megabits per second.
> >
> > **bitrate\_floor\_mbps** `integer` *required*
> >
> > Minimum acceptable average bitrate, in megabits per second.
> >
> > **bitrate\_target\_mbps** `integer` *required*
> >
> > Recommended average bitrate to encode at, in megabits per second.
> >
> > **camera\_lens** `string` *required*
> >
> > Which physical lens to record with.
> >
> > **codecs** `string[]` *required*
> >
> > Accepted video codecs, in preference order.
> >
> > **embed\_camera\_metadata** `boolean` *required*
> >
> > Whether the client must also write the camera make and model into the video
> > container's metadata. When `false`, the capture manifest and export CSV are
> > the metadata carrier.
> >
> > **fps** `integer` *required*
> >
> > Target capture frame rate.
> >
> > **frame\_gap\_tolerance\_ms** `integer` *required*
> >
> > Longest stall between consecutive frames a clip may contain before the client
> > rejects it, in milliseconds. Every frame is timestamped in the frame log, so a
> > stall stays alignable downstream — this bounds how broken a capture may be,
> > not how evenly it must be paced.
> >
> > **height** `integer` *required*
> >
> > Required frame height in pixels — recorded footage must match exactly.
> >
> > **min\_fov\_degrees** `integer` *required*
> >
> > Minimum acceptable horizontal field of view, in degrees.
> >
> > **orientation** `string` *required*
> >
> > Device orientation to record in.
> >
> > **preferred\_fov\_degrees** `integer` *required*
> >
> > Preferred horizontal field of view, in degrees.
> >
> > **stabilization\_mode** `string` *required*
> >
> > How the client must configure video stabilization: `off` disables EIS so raw motion is preserved for pose extraction, `on` requires it, `any` leaves the device default.
> >
> > Available options: `off`, `on`, `any`
> >
> > **stabilization\_required** `boolean` *required*
> >
> > Whether hardware/software stabilization must be enabled. True exactly when
> > stabilization\_mode is `on`.
> >
> > **width** `integer` *required*
> >
> > Required frame width in pixels — recorded footage must match exactly.

**created\_at** `string` *required*

When the bounty was created, as an ISO 8601 timestamp.

**currency** `string` *required*

Currency for all amounts on the bounty, as a lowercase ISO 4217 code.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `cny`, `kzt`, `awg`, `hnl`

**denied\_submissions\_count** `integer` *required*

Submissions reviewed and turned down.

**description** `string` *required*

Full task instructions shown to workers.

**discussion\_experience\_id** `string | null` *required*

Experience the bounty's discussion thread lives in, prefixed `exp_`. Read this
— not `experience_id` — to open the thread: a platform-wide bounty has no
hosting experience of its own but its discussion still lives in one.

**discussion\_feed\_id** `string | null` *required*

Forum feed containing the bounty's discussion thread. `null` for a bounty with
no forum post.

**discussion\_post\_id** `string | null` *required*

Forum post anchoring the bounty's discussion thread. Read together with
`discussion_experience_id` to address the thread. `null` for a bounty with no
forum post.

**experience\_id** `string | null` *required*

Experience the bounty is hosted in, prefixed `exp_`. `null` for platform-wide
bounties; may belong to a different account than the funder.

**funding\_account** `object | null` *required*

Account whose balance funds the bounty pool, or `null` when a user funds it personally. May differ from the account hosting `experience_id`.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **title** `string` *required*
>
> Account display name.

**gross\_paid\_out\_amount** `number` *required*

Gross amount paid out from the bounty pool across accepted submissions —
worker payouts, platform fees, and affiliate shares together. Tips and
reviewer rewards are excluded.

**gross\_reward\_amount** `number` *required*

Gross bounty-pool amount allocated per accepted submission, in whole currency
units.

**hosting\_account** `object | null` *required*

Account hosting the bounty's forum — the one whose `route` and `experience_id` address its discussion thread, and where its submissions dashboard lives. `null` for a platform-wide bounty with no host. May differ from `funding_account`.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **banner\_image\_url** `string | null` *required*
>
> Account banner image URL. `null` when the account has not set one.
>
> **logo\_url** `string | null` *required*
>
> Account logo image URL. `null` when the account has not set one.
>
> **route** `string` *required*
>
> Account public route identifier, used in the `whop.com/\{route}` storefront
> path.
>
> **title** `string` *required*
>
> Account display name.

**min\_total\_verified\_duration\_seconds** `integer | null` *required*

Total verified footage a submission must accumulate before it can be
submitted, in seconds. Always a whole number of hours. Present only on
`data_capture` bounties — it is what `net_reward_amount` pays for, so rate
displays divide by it. `null` for every other goal type.

**net\_reward\_amount** `number` *required*

What a worker is quoted per accepted submission after the platform fee, in
whole currency units. The exact post-fee figure, at the standard platform fee
rate — a worker who locked a different rate, or who arrived through an
affiliate link, is paid a different amount.

**poster** `object` *required*

User who posted the bounty — the account owner when created with an account API key.

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

**scheduled\_frequency** `string | null` *required*

How often the schedule creates a new bounty. Each occurrence is a separate bounty; the original is not republished.

Available options: `once`, `hourly`, `daily`, `weekly`, `monthly`

**scheduled\_publish\_at** `string | null` *required*

When a scheduled bounty will publish, as an ISO 8601 timestamp. `null` once
published, for bounties that were never scheduled, and for terminally failed
drafts parked for manual rescheduling.

**spots\_remaining** `integer` *required*

Unfilled winner capacity: `accepted_submissions_limit` minus
`accepted_submissions_count`, clamped to zero. Not on its own a signal that
the bounty accepts new claims — read `status` for that: only an `open` bounty
takes new submissions.

**status** `string` *required*

Lifecycle state. `scheduled` bounties are unpublished drafts, visible to their poster and the account's authorized managers; `open` bounties accept new submissions; `closed` bounties are live but no longer accept new submissions; `completed` bounties paid out every winner slot; `canceled` bounties ended before filling their slots.

Available options: `scheduled`, `open`, `closed`, `completed`, `canceled`

**submissions\_closed\_at** `string | null` *required*

When new submissions stopped being accepted, as an ISO 8601 timestamp. Set
when a cancellation is requested on a bounty with work in flight, so in-flight
submissions can resolve before the bounty cancels. `null` when submissions
were never stopped — including completed bounties that simply filled every
winner slot.

**title** `string` *required*

Short name of the task shown to workers.

**unresolved\_submissions\_count** `integer` *required*

Submissions still awaiting an outcome: in progress or pending review.

**updated\_at** `string` *required*

When the bounty was last updated, as an ISO 8601 timestamp.

**viewer\_accepted\_submissions\_count** `integer` *required*

How many winner slots the authenticated user has already won on this bounty.
Read against `accepted_submissions_per_user_limit` to show a worker their
remaining allowance. `0` when the request has no authenticated user.

```json Bounty theme={null}
{
	"id": "bnty_xxxxxxxxxxxxx",
	"title": "Clip our ceramic coating livestream",
	"description": "Record a 30-60 second clip of the ceramic coating reveal and post it with the campaign hashtag.",
	"status": "open",
	"currency": "usd",
	"gross_reward_amount": 25,
	"net_reward_amount": 17.5,
	"affiliate_share_amount": 3.5,
	"budget_amount": 125,
	"gross_paid_out_amount": 50,
	"accepted_submissions_limit": 5,
	"accepted_submissions_per_user_limit": 2,
	"accepted_submissions_count": 2,
	"viewer_accepted_submissions_count": 1,
	"unresolved_submissions_count": 1,
	"awaiting_review_submissions_count": 1,
	"denied_submissions_count": 1,
	"spots_remaining": 3,
	"business_goal_type": "clipping",
	"accepted_deliverable_types": ["content_url", "media"],
	"capture_spec": null,
	"min_total_verified_duration_seconds": null,
	"allowed_country_codes": ["US", "MX"],
	"submissions_closed_at": null,
	"cancel_requested_at": null,
	"scheduled_publish_at": null,
	"scheduled_frequency": null,
	"experience_id": "exp_xxxxxxxxxxxxx",
	"discussion_post_id": "post_xxxxxxxxxxxxx",
	"discussion_feed_id": "feed_xxxxxxxxxxxxx",
	"discussion_experience_id": "exp_xxxxxxxxxxxxx",
	"active_proof_livestream_feeds": [
		{
			"id": "lfeed_xxxxxxxxxxxxx",
			"title": "Clipping the ceramic coating reveal live",
			"host": {
				"id": "user_xxxxxxxxxxxx",
				"username": "clip_worker",
				"name": "Tanya Cole",
				"profile_picture": {
					"url": "https://img.whop.com/worker.png"
				},
				"badges": [
					{
						"type": "verified_partner"
					}
				]
			}
		}
	],
	"hosting_account": {
		"id": "biz_xxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing",
		"route": "shine-time-auto-detailing",
		"logo_url": "https://img.whop.com/shine-time-auto-detailing-logo.png",
		"banner_image_url": "https://img.whop.com/shine-time-auto-detailing-banner.png"
	},
	"poster": {
		"id": "user_xxxxxxxxxxxx",
		"username": "shinetimedetailing",
		"name": "Marcus Webb",
		"profile_picture": {
			"url": "https://img.whop.com/creator.png"
		},
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"funding_account": {
		"id": "biz_xxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"created_at": "2026-07-01T00:00:00.000Z",
	"updated_at": "2026-07-08T00:00:00.000Z"
}
```


## Related topics

- [Bounty Submissions API](https://docs.whop.com/api-reference/bounty-submissions/bounty-submission.md)
- [Create bounty](https://docs.whop.com/api-reference/legacy/bounties/create-bounty.md)
- [List bounties](https://docs.whop.com/api-reference/legacy/bounties/list-bounties.md)
