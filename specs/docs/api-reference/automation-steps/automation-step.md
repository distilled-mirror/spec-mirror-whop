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

# Automation Steps API

> An Automation step is one step in a version of an automation. A `wait` step holds a member for `config.delay_seconds`.

An `email` or `message` step runs its `source_code` to decide whether to send and what to say. Steps form a chain, and each step names the step before it. Only the draft version can change.

Use the Automation steps API to list a version's steps, and to add, update and remove steps in the draft. You can also preview what an `email` or `message` step would send a member. A preview sends nothing.

A step's code reads the account through the API. It holds every account-level `:read` permission that both the admin role and Whop AI hold, except `payout:account:read`. So it can't read cards. Setting a step's `source_code`, or previewing a step, needs every one of those permissions. So a credential can't write code that reads more than it can.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Automation Steps](https://docs.whop.com/api-reference/automation-steps/list-automation-steps.md) | GET `/automation_steps` |
| [Retrieve Automation Step](https://docs.whop.com/api-reference/automation-steps/retrieve-automation-step.md) | GET `/automation_steps/{id}` |
| [Create Automation Step](https://docs.whop.com/api-reference/automation-steps/create-automation-step.md) | POST `/automation_steps` |
| [Preview Automation Step](https://docs.whop.com/api-reference/automation-steps/preview-automation-step.md) | POST `/automation_steps/{id}/preview` |
| [Update Automation Step](https://docs.whop.com/api-reference/automation-steps/update-automation-step.md) | PATCH `/automation_steps/{id}` |
| [Delete Automation Step](https://docs.whop.com/api-reference/automation-steps/delete-automation-step.md) | DELETE `/automation_steps/{id}` |

## Attributes

**id** `string` *required*

Automation step ID, prefixed `atos_`. A step gets a new ID in every version.
Its `key` stays the same.

**action\_type** `string` *required*

What the step does: `wait` holds the member for `config.delay_seconds`, `email` emails them, and `message` sends them a message from a team member. A draft step can switch between `email` and `message`, which clears `config.sender_id`. A `wait` can't become a sending step, or the other way round.

Available options: `wait`, `email`, `message`

**automation\_id** `string` *required*

ID of the automation the step belongs to, prefixed `ato_`.

**config** `object` *required*

The step's settings for its `action_type`: `delay_seconds` for a `wait`, and
`sender_id` and `attachment_ids` for an `email` or `message`. Writing it
replaces the whole object, except that a step keeps its `attachment_ids` when
the write leaves them out.

**created\_at** `string` *required*

When the step was created, as an ISO 8601 timestamp.

**issues** `string[]` *required*

What must be fixed before the draft can be published, such as code that hasn't
been written. Empty when there's nothing.

**key** `integer` *required*

Identifies the same step across versions. A new step gets a key no step of the
automation has used.

**previous\_step\_id** `string | null` *required*

ID of the step this one runs after, prefixed `atos_`. `null` when it runs
first, directly after the member enrolls.

**sender** `object | null` *required*

The sender `config.sender_id` names. `null` on a `wait`, and when the step
sends from the default sender.

**source\_code** `string | null` *required*

An `email` or `message` step's code: a TSX module whose default export gets
the member and returns `message.send()`, `message.skip()` or `message.exit()`
from `whop:message`. An `email` step sends its `subject`, its `react` element
as the body and its `text` as the plain-text part. A `message` step sends its
`text`. The code can import `react` and `@react-email/components`. It reads
the account through the `whop` client from `whop:message`, which can only
read. That client holds every account-level `:read` permission that both the
admin role and Whop AI hold, except `payout:account:read`, as listed for
previewing a step. Setting it, or previewing the step, needs every one of
those permissions. Up to 64,000 characters. `null` on a `wait`, and until it
is written.

**updated\_at** `string` *required*

When the step was last updated, as an ISO 8601 timestamp.

**version** `integer` *required*

The automation version this step is part of. Only the draft version's steps
can change.

**waiting\_enrollments\_count** `integer` *required*

Members on this step's version who are waiting for it to run.

```json AutomationStep theme={null}
{
	"id": "atos_xxxxxxxxxxxxxx",
	"automation_id": "ato_xxxxxxxxxxxxxx",
	"version": 3,
	"key": 2,
	"previous_step_id": "atos_yyyyyyyyyyyyyy",
	"action_type": "email",
	"config": {
		"sender_id": "emaddr_xxxxxxxxxxxxxx",
		"attachment_ids": []
	},
	"sender": {
		"id": "emaddr_xxxxxxxxxxxxxx",
		"type": "email_address",
		"address": "support@shinetime.example"
	},
	"source_code": "export default () => message.send({ subject: \"Thanks for booking!\", text: \"See you on Saturday.\" });",
	"issues": [],
	"waiting_enrollments_count": 5,
	"created_at": "2026-10-01T15:30:00.000Z",
	"updated_at": "2026-10-02T08:45:00.000Z"
}
```


## Related topics

- [Automations API](https://docs.whop.com/api-reference/automations/automation.md)
- [List Automation Steps](https://docs.whop.com/api-reference/automation-steps/list-automation-steps.md)
- [Delete Automation Step](https://docs.whop.com/api-reference/automation-steps/delete-automation-step.md)
