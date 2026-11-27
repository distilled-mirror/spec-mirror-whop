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

# Recommended Actions API

> A recommended action is one change that Whop recommends for an account with Economic Intelligence. Its `workflow` lists the API calls it makes when it runs.

Use the Recommended Actions API to list an account's actions, run a `ready` action, or dismiss it. A run sends each step in order, as the user who runs it. Retrieve the action to follow each step's `status`.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Recommended Actions](https://docs.whop.com/api-reference/recommended-actions/list-recommended-actions.md) | GET `/recommended_actions` |
| [Retrieve Recommended Action](https://docs.whop.com/api-reference/recommended-actions/retrieve-recommended-action.md) | GET `/recommended_actions/{id}` |
| [Dismiss Recommended Action](https://docs.whop.com/api-reference/recommended-actions/dismiss-recommended-action.md) | POST `/recommended_actions/{id}/dismiss` |
| [Execute Recommended Action](https://docs.whop.com/api-reference/recommended-actions/execute-recommended-action.md) | POST `/recommended_actions/{id}/execute` |
| [Update Recommended Action](https://docs.whop.com/api-reference/recommended-actions/update-recommended-action.md) | PATCH `/recommended_actions/{id}` |

## Attributes

**id** `string` *required*

Recommended action ID, prefixed `reca_`.

**account\_id** `string` *required*

The account the action is for, prefixed `biz_`.

**created\_at** `string` *required*

When the action was created, as an ISO 8601 timestamp.

**experiment\_id** `string | null` *required*

The experiment measuring the action's outcome, prefixed `expt_`. `null` when
there is none.

**preview** `object | null` *required*

The website change the action makes when it runs. `type` picks the shape. New
types can be added, so skip a preview whose `type` you do not know. `null`
while the action is `preparing`, or when the action does not change a website.

**reasoning** `string` *required*

Why Whop recommends the action, written for the account owner.

**run\_by\_user\_id** `string | null` *required*

The user who ran the action, prefixed `user_`. `null` until the action runs.

**run\_ended\_at** `string | null` *required*

When the run ended, as an ISO 8601 timestamp. `null` until the run ends.

**run\_started\_at** `string | null` *required*

When the run started, as an ISO 8601 timestamp. `null` until the action runs.

**status** `string` *required*

Where the action is. `preparing` while its work is built, and it cannot run yet. `ready` when it can run. `running` while its steps are sent. `executed` when every step succeeded. `failed` when the run ended before every step succeeded. `dismissed` when the owner chose not to run it. `superseded` when Whop retired it, such as for a newer action.

Available options: `preparing`, `ready`, `running`, `executed`, `failed`, `dismissed`, `superseded`

**target\_stats** `object[]` *required*

The measures the action sets out to move, the primary first, as the stats API reads them.

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

**title** `string` *required*

What the action does, written for the account owner.

**type** `string` *required*

What kind of change the action makes. New values can be added, so skip an action whose type you do not know.

Available options: `add_cancel_offer`, `create_message_automation`, `improve_landing_page`, `fix_broken_website`, `clone_and_ab_test_site`

**updated\_at** `string` *required*

When the action was last updated, as an ISO 8601 timestamp.

**workflow** `object | null` *required*

The API calls the action makes when it runs. `null` while the action is `preparing`, or when it has no steps.

> [accordion] Properties
>
> **api\_version** `string` *required*
>
> The `Api-Version-Date` every step is sent with, such as `2026-11-14`. The
> arguments of each step have the shape of this version.
>
> **steps** `object[]` *required*
>
> The API calls the action makes when it runs, in the order they are sent.
>
> > [accordion] Properties
> >
> > **arguments** `object` *required*
> >
> > The input of the operation under `api_version`. It holds the path, query and body parameters in one map. A value of the form `\{\{steps[\<index>].\<field>}}` reads a field from the response of an earlier step. `\<index>` is that step's position in `steps`, counting from 0. The value is replaced before the step is sent.
> >
> > **error** `string | null` *required*
> >
> > Why the call did not go through. `null` when the step succeeded or was not
> > sent.
> >
> > **operation** `string` *required*
> >
> > The `operationId` of the API operation the step calls, such as
> > `createExperiment`.
> >
> > **status** `string` *required*
> >
> > Where the step is in the run. `pending` until the run sends it. `skipped` means an earlier step failed, so this step was not sent.
> >
> > Available options: `pending`, `running`, `succeeded`, `failed`, `skipped`
> >
> > **title** `string` *required*
> >
> > One sentence for the account owner about what the step changes, such as `Add a one-time wash button in the hero`.

```json RecommendedAction theme={null}
{
	"id": "reca_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"type": "improve_landing_page",
	"title": "Lead your home page with before and after detailing photos",
	"reasoning": "Visitors who reach your pricing section buy at 4.1%, but 62% leave the home page before they scroll to it. Showing before and after detailing photos first helps drivers see the finish.",
	"status": "ready",
	"workflow": {
		"api_version": "2026-11-14",
		"steps": [
			{
				"title": "Offer the Interior Deep Clean package on the website",
				"operation": "updatePlan",
				"arguments": {
					"id": "plan_xxxxxxxxxxxxxx",
					"visibility": "hidden"
				},
				"status": "pending",
				"error": null
			},
			{
				"title": "Offer the Ceramic Coating Package on the website",
				"operation": "updatePlan",
				"arguments": {
					"id": "plan_yyyyyyyyyyyyyy",
					"visibility": "hidden"
				},
				"status": "pending",
				"error": null
			}
		]
	},
	"run_by_user_id": null,
	"run_started_at": null,
	"run_ended_at": null,
	"created_at": "2026-11-17T05:12:44.318Z",
	"updated_at": "2026-11-17T05:14:02.907Z",
	"preview": {
		"type": "website_ab_test",
		"app_id": "app_xxxxxxxxxxxxxx",
		"base_build_id": "apbu_xxxxxxxxxxxxxx",
		"draft_build_id": "apbu_yyyyyyyyyyyyyy",
		"page_paths": ["/"],
		"changed_resources": [
			{
				"id": "plan_xxxxxxxxxxxxxx",
				"resource_type": "variant"
			},
			{
				"id": "plan_yyyyyyyyyyyyyy",
				"resource_type": "variant"
			}
		],
		"traffic_percent": 50
	},
	"experiment_id": null,
	"target_stats": [
		{
			"metric": "gross_revenue",
			"params": {
				"from": "2026-11-10",
				"to": "2026-11-16",
				"product": "prod_xxxxxxxxxxxxxx"
			},
			"estimate": 1240,
			"direction": "up"
		}
	]
}
```


## Related topics

- [Retrieve Recommended Action](https://docs.whop.com/api-reference/recommended-actions/retrieve-recommended-action.md)
- [Execute Recommended Action](https://docs.whop.com/api-reference/recommended-actions/execute-recommended-action.md)
- [Dismiss Recommended Action](https://docs.whop.com/api-reference/recommended-actions/dismiss-recommended-action.md)
