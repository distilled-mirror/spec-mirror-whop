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

# Automation Enrollments API

> An Automation enrollment is one member's pass through an automation.

It records the version the member is on, the step they reach next and when, and whether they are still going, finished, or canceled.

Use the Automation enrollments API to list who is in an automation, or which automations a member is in. You can also retrieve one enrollment, and cancel one to stop that member's remaining steps.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Automation Enrollments](https://docs.whop.com/api-reference/automation-enrollments/list-automation-enrollments.md) | GET `/automation_enrollments` |
| [Retrieve Automation Enrollment](https://docs.whop.com/api-reference/automation-enrollments/retrieve-automation-enrollment.md) | GET `/automation_enrollments/{id}` |
| [Cancel Automation Enrollment](https://docs.whop.com/api-reference/automation-enrollments/cancel-automation-enrollment.md) | POST `/automation_enrollments/{id}/cancel` |

## Attributes

**id** `string` *required*

Automation enrollment ID, prefixed `atoe_`.

**account\_id** `string` *required*

ID of the account the automation belongs to, prefixed `biz_`.

**automation\_id** `string` *required*

ID of the automation, prefixed `ato_`.

**created\_at** `string` *required*

When the member enrolled, as an ISO 8601 timestamp.

**ended\_at** `string | null` *required*

When the enrollment ended, as an ISO 8601 timestamp. `null` while it's active.

**member\_id** `string` *required*

ID of the member in the automation, prefixed `mber_`.

**next\_run\_at** `string | null` *required*

When that step runs for this member, as an ISO 8601 timestamp. `null` once the
enrollment ends.

**next\_step\_id** `string | null` *required*

ID of that step in the member's version, prefixed `atos_`. `null` once the
enrollment ends.

**next\_step\_key** `integer | null` *required*

The `key` of the step the member is waiting on, in their version. `null` once
the enrollment ends.

**status** `string` *required*

`active` while the member moves through the automation. `completed` after its last step. `canceled` when the account canceled the enrollment, or stopped or deleted the automation.

Available options: `active`, `completed`, `canceled`

**trigger\_event** `string | null` *required*

The webhook event that enrolled the member. `null` when `POST /automations/\{id}/trigger` enrolled them.

Available options: `payment.succeeded`, `payment.failed`, `membership.activated`, `membership.trial_ending_soon`, `membership.cancel_at_period_end_changed`, `member.created`, `invoice.created`, `invoice.paid`, `invoice.past_due`, `refund.created`, `dispute.created`, `resolution_center_case.created`, `resolution_center_case.decided`, `course_lesson_interaction.completed`

**trigger\_resource\_id** `string | null` *required*

ID of the record the triggering event was about, such as a `pay_` payment or
`mem_` membership. `null` when a trigger request enrolled them.

**updated\_at** `string` *required*

When the enrollment last changed, as an ISO 8601 timestamp.

**user\_id** `string | null` *required*

ID of the member's user, prefixed `user_`. `null` when the member is another
business.

**version** `integer` *required*

The automation version the member is on. It changes only when a publish moves
members to the new version.

```json AutomationEnrollment theme={null}
{
	"id": "atoe_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"automation_id": "ato_xxxxxxxxxxxxxx",
	"member_id": "mber_xxxxxxxxxxxxxx",
	"user_id": "user_xxxxxxxxxxxxxx",
	"status": "active",
	"version": 2,
	"next_step_key": 2,
	"next_step_id": "atos_xxxxxxxxxxxxxx",
	"next_run_at": "2026-10-15T09:00:00.000Z",
	"trigger_event": "payment.succeeded",
	"trigger_resource_id": "pay_xxxxxxxxxxxxxx",
	"ended_at": null,
	"created_at": "2026-10-14T09:00:00.000Z",
	"updated_at": "2026-10-14T09:00:05.000Z"
}
```


## Related topics

- [Automations API](https://docs.whop.com/api-reference/automations/automation.md)
- [Retrieve Automation Enrollment](https://docs.whop.com/api-reference/automation-enrollments/retrieve-automation-enrollment.md)
- [Cancel Automation Enrollment](https://docs.whop.com/api-reference/automation-enrollments/cancel-automation-enrollment.md)
