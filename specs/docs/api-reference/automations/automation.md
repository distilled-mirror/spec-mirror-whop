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

# Automations API

> An Automation is a chain of steps that members move through one at a time.

A member enrolls when the automation's `trigger_event` happens to them, such as `payment.succeeded`. You can also enroll a member with `POST /automations/{id}/trigger`. A `wait` step holds them for a set time. An `email` or `message` step runs its `source_code`, a TSX module that reads the member's data through the API. The module decides whether to send and what to say.

Steps are versioned. You edit the draft version. `POST /automations/{id}/publish` makes it the version new members start on, then copies it into the next draft. Each publish moves members already in the automation onto the new version. Use the Automations API to create, publish and trigger automations, and to update them, including their status. Use the Automation steps and Automation enrollments APIs for their steps and members.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Automations](https://docs.whop.com/api-reference/automations/list-automations.md) | GET `/automations` |
| [Retrieve Automation](https://docs.whop.com/api-reference/automations/retrieve-automation.md) | GET `/automations/{id}` |
| [Create Automation](https://docs.whop.com/api-reference/automations/create-automation.md) | POST `/automations` |
| [Publish Automation](https://docs.whop.com/api-reference/automations/publish-automation.md) | POST `/automations/{id}/publish` |
| [Trigger Automation](https://docs.whop.com/api-reference/automations/trigger-automation.md) | POST `/automations/{id}/trigger` |
| [Update Automation](https://docs.whop.com/api-reference/automations/update-automation.md) | PATCH `/automations/{id}` |
| [Delete Automation](https://docs.whop.com/api-reference/automations/delete-automation.md) | DELETE `/automations/{id}` |

## Attributes

**id** `string` *required*

Automation ID, prefixed `ato_`.

**account\_id** `string` *required*

ID of the account the automation belongs to, prefixed `biz_`.

**active\_enrollments\_count** `integer` *required*

Members currently in the automation.

**created\_at** `string` *required*

When the automation was created, as an ISO 8601 timestamp.

**created\_by\_actor** `object` *required*

Who created the automation.

> [accordion] Properties
>
> **id** `string` *required*
>
> ID of the user, account or app, prefixed `user_`, `biz_` or `app_`.
>
> **resource\_type** `string` *required*
>
> What acted: a `user`, an `account` through one of its API keys, or an `app`.
>
> Available options: `user`, `account`, `app`

**draft\_version** `integer` *required*

The version that step edits change. Always `published_version` plus one, or
`1` before the first publish.

**has\_unpublished\_changes** `boolean` *required*

Whether the draft has edits that `POST /automations/\{id}/publish` would
publish. Always `true` before the first publish.

**name** `string` *required*

The automation's name, shown on the dashboard. Up to 255 characters.

**published\_at** `string | null` *required*

When the automation was last published, as an ISO 8601 timestamp. `null` until
its first publish.

**published\_by\_actor** `object | null` *required*

Who last published the automation. `null` until its first publish.

> [accordion] Properties
>
> **id** `string` *required*
>
> ID of the user, account or app, prefixed `user_`, `biz_` or `app_`.
>
> **resource\_type** `string` *required*
>
> What acted: a `user`, an `account` through one of its API keys, or an `app`.
>
> Available options: `user`, `account`, `app`

**published\_version** `integer | null` *required*

The version new members start on. `null` until the automation is first
published.

**secret\_names** `string[]` *required*

Names of the automation's secrets, which its steps' code reads as environment
variables. Their values are never returned.

**status** `string` *required*

`active` enrolls members and runs their steps. `closed` enrolls no one new, and members already in it finish. `stopped` enrolls no one, and its members' enrollments have ended. A new automation is `stopped` until its first publish makes it `active`.

Available options: `active`, `closed`, `stopped`

**total\_enrollments\_count** `integer` *required*

How many times members have enrolled in the automation, including enrollments
that have ended.

**trigger\_event** `string | null` *required*

The webhook event that enrolls the member it's about, such as `payment.succeeded`. `null` for an automation that only `POST /automations/\{id}/trigger` enrolls into. Fixed once the automation has been published.

Available options: `payment.succeeded`, `payment.failed`, `membership.activated`, `membership.trial_ending_soon`, `membership.cancel_at_period_end_changed`, `member.created`, `invoice.created`, `invoice.paid`, `invoice.past_due`, `refund.created`, `dispute.created`, `resolution_center_case.created`, `resolution_center_case.decided`, `course_lesson_interaction.completed`

**updated\_at** `string` *required*

When the automation was last updated, as an ISO 8601 timestamp.

```json Automation theme={null}
{
	"id": "ato_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"name": "Welcome new customers",
	"status": "active",
	"trigger_event": "payment.succeeded",
	"published_version": 2,
	"draft_version": 3,
	"has_unpublished_changes": false,
	"published_at": "2026-10-01T15:30:00.000Z",
	"published_by_actor": {
		"id": "user_yyyyyyyyyyyyyy",
		"resource_type": "user"
	},
	"created_by_actor": {
		"id": "user_yyyyyyyyyyyyyy",
		"resource_type": "user"
	},
	"secret_names": ["SHOPIFY_TOKEN"],
	"active_enrollments_count": 42,
	"total_enrollments_count": 318,
	"created_at": "2026-09-12T10:00:00.000Z",
	"updated_at": "2026-10-01T15:30:00.000Z"
}
```


## Related topics

- [Automation Steps API](https://docs.whop.com/api-reference/automation-steps/automation-step.md)
- [Automation Enrollments API](https://docs.whop.com/api-reference/automation-enrollments/automation-enrollment.md)
- [Delete Automation](https://docs.whop.com/api-reference/automations/delete-automation.md)
