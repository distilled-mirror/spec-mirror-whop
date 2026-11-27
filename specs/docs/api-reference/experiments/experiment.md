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

# Experiments API

> An Experiment is a feature flag or A/B test owned by an account.

Treatments take stable percentage ranges of traffic and everyone else gets control, so growing an allocation never moves an existing user to another arm.

Use the Experiments API to create a draft, configure its weights, targeting, and resource bindings, then activate, pause, or end it, and to evaluate which arm a user, account, or anonymous visitor gets. Managing experiments requires `experiment:read` or `experiment:manage`; evaluation works without authentication.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Experiments](https://docs.whop.com/api-reference/experiments/list-experiments.md) | GET `/experiments` |
| [Retrieve Experiment](https://docs.whop.com/api-reference/experiments/retrieve-experiment.md) | GET `/experiments/{id}` |
| [Get Experiment Results](https://docs.whop.com/api-reference/experiments/get-experiment-results.md) | GET `/experiments/{id}/results` |
| [Evaluate Experiments](https://docs.whop.com/api-reference/experiments/evaluate-experiments.md) | GET `/experiments/exposures` |
| [Create Experiment](https://docs.whop.com/api-reference/experiments/create-experiment.md) | POST `/experiments` |
| [Activate Experiment](https://docs.whop.com/api-reference/experiments/activate-experiment.md) | POST `/experiments/{id}/activate` |
| [End Experiment](https://docs.whop.com/api-reference/experiments/end-experiment.md) | POST `/experiments/{id}/end` |
| [Pause Experiment](https://docs.whop.com/api-reference/experiments/pause-experiment.md) | POST `/experiments/{id}/pause` |
| [Update Experiment](https://docs.whop.com/api-reference/experiments/update-experiment.md) | PATCH `/experiments/{id}` |

## Attributes

**id** `string` *required*

Unique identifier for the experiment, prefixed `expt_`.

**account\_id** `string` *required*

Owning account ID.

**assignment\_seed** `string` *required*

Assignment hashes UTF-8 seed + subject ID with CRC32 modulo 100 and selects
the stored end-exclusive range.

**bucket\_by** `string | null`

Randomization unit — `user` buckets each user independently, `account` buckets whole accounts (every user of an account gets the same arm). `null` for feature flags.

Available options: `user`, `account`, `anonymous`

**configuration\_revision** `integer` *required*

Revision of the serving configuration. Increments on every configuration
change, so a cached definition with a lower revision is stale. Does not change
the assignment seed.

**control** `object` *required*

> [accordion] Properties
>
> **related\_resource** `object | null`
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Referenced resource tag, belonging to the experiment owner.
> >
> > **object** `string` *required*
> >
> > Available options: `app`, `app_build`, `product`, `plan`

**created\_at** `string | null`

When the experiment was created, as an ISO 8601 timestamp.

**created\_by** `string | null`

ID of the user who created the experiment, prefixed `user_`. `null` for
experiments created before creators were recorded.

**decision\_mode** `string | null`

How the experiment is decided. `autopilot` ends it as soon as a result is outside the margin of error, shipping the arm that won or keeping control if it lost. If its window passes with no clear result, `autopilot` keeps control. `require_approval` leaves every arm serving for the owner to end. `null` leaves it to the owner.

Available options: `autopilot`, `require_approval`

**ended\_at** `string | null`

When the experiment stopped collecting data, as an ISO 8601 timestamp. `null`
while still running.

**findings** `string | null`

What was learned and why this outcome, recorded when the experiment was ended.
`null` until then.

**flag\_key** `string` *required*

Developer-chosen handle referenced from code. Anywhere the API takes an
experiment identifier, the `expt_` id and the flag\_key are interchangeable.

**hypothesis** `string | null`

Hypothesis for this experiment. `null` when none is set, and always `null` for
feature flags.

**kind** `string`

The kind of experiment: `boolean` is a feature flag, `experiment` assigns its subjects to arms, `beforeafter` compares a change with the period before it.

Available options: `boolean`, `experiment`, `beforeafter`

**metadata** `object`

Key-value pairs attached when the experiment was created. Empty when none were
set.

**name** `string` *required*

Human-readable display name.

**related\_resource** `object | null` *required*

Resource owned by the account that this experiment is bound to, such as an app or product. `null` when unbound. Fixed once the experiment first activates.

> [accordion] Properties
>
> **id** `string` *required*
>
> Referenced resource tag, belonging to the experiment owner.
>
> **object** `string` *required*
>
> Available options: `app`, `app_build`, `product`, `plan`

**started\_at** `string | null`

When the experiment began collecting data, as an ISO 8601 timestamp. `null`
for drafts.

**status** `string` *required*

Lifecycle state. `draft` — not yet live; `active` — currently running; `paused` — traffic paused; `ended` — concluded.

Available options: `draft`, `active`, `paused`, `ended`

**target\_stats** `object[] | null`

The measures the experiment sets out to move, as the stats API reads them. `null` when there are none.

> [accordion] Properties
>
> **direction** `string` *required*
>
> Which way the measure should move.
>
> Available options: `up`, `down`
>
> **estimate** `number | null` *required*
>
> The value the measure is expected to reach, in its own unit. `null` when there
> is no estimate.
>
> **metric** `string` *required*
>
> The stats metric, the key of `GET /stats/time_series/\{metric}`.
>
> **params** `object` *required*
>
> The parameters of that metric beyond the account, as the stats API takes them.

**updated\_at** `string | null` *required*

**variants** `object[]` *required*

Treatment arms. Users outside every arm's allocation form the implicit `control` group. Weights only ever grow and arms are never removed, so a user moves from control into a treatment at most once.

> [accordion] Properties
>
> **name** `string` *required*
>
> Treatment identifier. `control` is reserved — it is the implicit remainder.
>
> **ranges** `integer[][]`
>
> Granted bucket ranges (1% units, end-exclusive) recording this arm's
> allocation history. Server-managed; ranges are only ever appended, which is
> what keeps assignments permanent.
>
> **related\_resource** `object | null`
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Referenced resource tag, belonging to the experiment owner.
> >
> > **object** `string` *required*
> >
> > Available options: `app`, `app_build`, `product`, `plan`
>
> **weight** `integer` *required*
>
> Percentage of all users assigned to this treatment, 1–100. All weights together sum to at most 100; the remainder is control.

**winning\_arm** `string | null`

The treatment that won, set when the experiment was ended. Once set, every
evaluation returns this arm to every caller regardless of targeting or
allocation, and no further exposures are recorded. `null` means control won —
an ended experiment with no winning arm evaluates to `control` for everyone.
Always `null` for feature flags, which simply evaluate to disabled once ended.

```json Experiment theme={null}
{
	"id": "expt_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"flag_key": "checkout_redesign_v2",
	"name": "Checkout Redesign V2",
	"hypothesis": "A single-step checkout raises conversion.",
	"status": "active",
	"bucket_by": "user",
	"assignment_seed": "checkout_redesign_v21789462000",
	"configuration_revision": 2,
	"variants": [
		{
			"name": "treatment",
			"weight": 50,
			"ranges": [[0, 50]],
			"related_resource": null
		}
	],
	"control": {
		"related_resource": null
	},
	"related_resource": null,
	"winning_arm": null,
	"findings": null,
	"created_by": "user_xxxxxxxxxxxxxx",
	"created_at": "2026-09-01T12:00:00.000Z",
	"started_at": "2026-09-02T09:00:00.000Z",
	"ended_at": null,
	"updated_at": "2026-09-02T09:00:00.000Z"
}
```


## Related topics

- [Evaluate Experiments](https://docs.whop.com/api-reference/experiments/evaluate-experiments.md)
- [Pause Experiment](https://docs.whop.com/api-reference/experiments/pause-experiment.md)
- [Create Experiment](https://docs.whop.com/api-reference/experiments/create-experiment.md)
