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

# Team Members API

> A Team Member is a member of an account's team: the link between a user and an account, carrying the role that controls what they can do.

Roles are either system roles (like `admin` or `moderator`) or `custom` roles managed from the dashboard.

Use the Team Members API to list an account's team, add a user to the team with a system role, change a member's role, and remove members. Adding a user who has not yet accepted sends an invitation instead.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Team Members](https://docs.whop.com/api-reference/team-members/list-team-members.md) | GET `/team_members` |
| [Retrieve Team Member](https://docs.whop.com/api-reference/team-members/retrieve-team-member.md) | GET `/team_members/{id}` |
| [Create Team Member](https://docs.whop.com/api-reference/team-members/create-team-member.md) | POST `/team_members` |
| [Update Team Member](https://docs.whop.com/api-reference/team-members/update-team-member.md) | PATCH `/team_members/{id}` |
| [Delete Team Member](https://docs.whop.com/api-reference/team-members/delete-team-member.md) | DELETE `/team_members/{id}` |

## Attributes

**id** `string` *required*

Team member ID. Accepted members have an ID starting with `ausr_` or `mber_`,
and pending invites one starting with `ausri_`. Treat it as opaque: both
member prefixes are accepted wherever a team member ID is.

**account** `object` *required*

The account this membership belongs to.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **fees** `object`
>
> Markup rates this parent charges the subaccount being read, keyed by fee type
> (for example `crypto_deposit_markup`), each with `percentage_fee` and
> `fixed_fee_usd`. Resolved with the subaccount's own overrides winning over the
> platform default.
>
> **logo\_url** `string | null` *required*
>
> Account logo image URL.
>
> **route** `string` *required*
>
> Account public route identifier.
>
> **title** `string` *required*
>
> Account display name.

**authorized\_role** `object | null` *required*

Custom role assigned to this member, or `null` when the member has a system role.

> [accordion] Properties
>
> **id** `string` *required*
>
> Custom role ID, prefixed `aurl_`.
>
> **name** `string` *required*
>
> Custom role name.

**created\_at** `string` *required*

When the member joined the team or the invite was sent, as an ISO 8601
timestamp. A member who was a customer first keeps the date they were given
their role.

**direct\_reports\_count** `integer | null` *required*

How many team members report directly to this member. `null` for invites.

**email** `string | null` *required*

The member's email address. For an invite to an email with no Whop account
yet, this is the invited address. Otherwise it is `null` unless the caller
holds the `company:authorized_user:email:read` scope.

**is\_agent** `boolean` *required*

Whether this member is an agent (app-controlled account) rather than a human
team member. Always `false` for invites.

**is\_partner** `boolean` *required*

Whether this member or invite has the view-only Partner role, directly or
through a custom role based on it. A Partner can read only the account's basic
details.

**legal\_name** `string | null` *required*

The first and last name on the member's approved identity verification, set
only when the member has no profile name. Returned to the account's own API
key when listing or retrieving a partner account's team members. `null` when
the member has a profile name or no approved verification, for invites, on
write responses, and for other callers.

**reports\_to\_chain** `object[] | null` *required*

The managers above this member, nearest first: the member they report to, then that member's manager, up to the top of the reporting line. `[]` when the member reports to the business directly. A caller who may read only their own reporting line sees it end at themselves, and `null` where it is not readable to them. `null` for invites.

> [accordion] Properties
>
> **id** `string` *required*
>
> The manager's team member ID, starting with `ausr_` or `mber_`; treat it as
> opaque.
>
> **legal\_name** `string | null` *required*
>
> The first and last name on the manager's approved identity verification, set
> only when the manager has no profile name. Returned to the account's own API
> key when listing a partner account's team. `null` when the manager has a
> profile name or no approved verification, and for other callers.
>
> **user** `object` *required*
>
> The user behind the manager's team membership.
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

**reports\_to\_id** `string | null` *required*

The ID of the team member this member reports to, starting with `ausr_` or
`mber_`; treat it as opaque. `null` when the member reports to the business
directly, and for invites.

**role** `string` *required*

The member's role on the account. `custom` means a bespoke dashboard-managed role; the API can read but not grant it.

Available options: `owner`, `admin`, `partner`, `sales_manager`, `moderator`, `advertiser`, `app_manager`, `support`, `manager`, `workforce`, `custom`

**status** `string` *required*

`joined` for accepted members, `pending` while the invite is pending.

Available options: `joined`, `pending`

**updated\_at** `string` *required*

When the membership was last updated, as an ISO 8601 timestamp.

**user** `object | null` *required*

The user behind this team membership. `null` for an invite sent to an email with no Whop account yet.

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

```json TeamMember theme={null}
{
	"id": "ausr_xxxxxxxxxxxxx",
	"status": "joined",
	"role": "admin",
	"authorized_role": null,
	"is_agent": false,
	"is_partner": false,
	"account": {
		"id": "biz_xxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing",
		"route": "shine-time-auto-detailing",
		"logo_url": "https://cdn.whop.com/logo.png"
	},
	"email": "dana@shinetime.example",
	"created_at": "2026-07-01T00:00:00.000Z",
	"updated_at": "2026-07-01T00:00:00.000Z",
	"reports_to_id": "ausr_xxxxxxxxxxxxy",
	"direct_reports_count": 0,
	"legal_name": null,
	"reports_to_chain": [
		{
			"id": "ausr_xxxxxxxxxxxxy",
			"legal_name": "Samuel Lee",
			"user": {
				"id": "user_xxxxxxxxxxxy",
				"name": "",
				"profile_picture": {
					"url": "https://cdn.whop.com/manager.png"
				},
				"username": "samlee",
				"badges": [
					{
						"type": "verified_partner"
					}
				]
			}
		}
	],
	"user": {
		"id": "user_xxxxxxxxxxxx",
		"name": "Dana Whitfield",
		"profile_picture": {
			"url": "https://cdn.whop.com/user.png"
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

- [Permissions API](https://docs.whop.com/api-reference/permissions/permission.md)
- [Update Team Member](https://docs.whop.com/api-reference/team-members/update-team-member.md)
- [List Team Members](https://docs.whop.com/api-reference/team-members/list-team-members.md)
