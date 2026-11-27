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

# Members API

> A Member is one buyer's relationship with an account: one record per customer, however many memberships they hold.

It tracks whether they have joined or left, their access level, and when they last opened the account's content.

Use the Members API to list and search an account's members, retrieve a single member, and read a member's activity log. Member rows are created and maintained by the membership lifecycle; to grant or revoke access, work with memberships instead.

> [!NOTE]
>
> Replaces the Legacy [Members](https://docs.whop.com/api-reference/members/member) resource.
> Existing Legacy integrations keep working; see [API
> versions](https://docs.whop.com/developer/api/versioning.md) for the stability contract.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Members](https://docs.whop.com/api-reference/members/list-members.md) | GET `/members` |
| [Retrieve Member](https://docs.whop.com/api-reference/members/retrieve-member.md) | GET `/members/{id}` |
| [List Member Logs](https://docs.whop.com/api-reference/members/list-member-logs.md) | GET `/members/{id}/logs` |
| [List Member Notes](https://docs.whop.com/api-reference/members/list-member-notes.md) | GET `/members/{member_id}/notes` |
| [Create Member](https://docs.whop.com/api-reference/members/create-member.md) | POST `/members` |
| [Create Member Note](https://docs.whop.com/api-reference/members/create-member-note.md) | POST `/members/{member_id}/notes` |
| [Unsubscribe Member](https://docs.whop.com/api-reference/members/unsubscribe-member.md) | POST `/members/{id}/unsubscribe` |
| [Update Member](https://docs.whop.com/api-reference/members/update-member.md) | PATCH `/members/{id}` |
| [Delete Member Note](https://docs.whop.com/api-reference/members/delete-member-note.md) | DELETE `/members/{member_id}/notes/{id}` |

## Attributes

**id** `string` *required*

Member ID, prefixed `mber_`.

**access\_level** `string` *required*

What the member can reach on the account: `customer` for paying members, `admin` for team members, `no_access` once every grant has lapsed.

Available options: `no_access`, `admin`, `customer`

**account\_id** `string` *required*

The account this member belongs to, prefixed `biz_`.

**buyer\_account\_id** `string | null` *required*

The business that bought, prefixed `biz_`. `null` when the buyer is a person.

**created\_at** `string` *required*

When the member record was created, as an ISO 8601 timestamp.

**display\_name** `string` *required*

The name to show for this member: the user's name, or their username when they
have not set one. The buying business's name when `user` is `null`.

**earnings\_usd** `object | null` *required*

The user's gross income across Whop, in USD, refreshed periodically. `null` when the user hides their earnings or the buyer is another business.

> [accordion] Properties
>
> **last\_24\_hours** `string` *required*
>
> Gross income in USD over the last 24 hours.
>
> **last\_30\_days** `string` *required*
>
> Gross income in USD over the last 30 days.
>
> **last\_7\_days** `string` *required*
>
> Gross income in USD over the last 7 days.
>
> **lifetime** `string` *required*
>
> All-time gross income in USD.

**email** `string | null` *required*

The user's email address. `null` without `member:email:read` on the account,
when the buyer is another business, or when the user has no assigned email.

**joined\_at** `string` *required*

When the member first joined the account, as an ISO 8601 timestamp.

**last\_accessed\_at** `string | null` *required*

When the member last opened the account's content, as an ISO 8601 timestamp.
`null` if they never have.

**last\_activity** `object | null` *required*

What happened at `last_activity_at`: a message in the member's support chat. `type` says which kind of activity it is. `null` without `support_chat:read` on the account, or when `last_activity_at` is `null`.

> [accordion] Properties
>
> **id** `string` *required*
>
> The message ID.
>
> **author** `object | null` *required*
>
> Who sent the message: the member, or someone on the account's team. `null` when the sender's account no longer exists.
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
> **channel\_id** `string` *required*
>
> The support chat the message was sent in.
>
> **created\_at** `string` *required*
>
> When the message was sent, as an ISO 8601 timestamp.
>
> **plain\_content** `string | null` *required*
>
> The message text without formatting. `null` when the message has no text, such
> as an attachment on its own.
>
> **type** `string` *required*
>
> Always `message`.
>
> Available options: `message`

**last\_activity\_at** `string | null` *required*

When the member and the account last interacted, as an ISO 8601 timestamp. For
example, a message in the member's support chat.

**metadata** `object | null` *required*

Your own key-value data stored on this member. `null` when none is set.

**most\_recent\_action** `string | null` *required*

The member's latest lifecycle state on the account, derived from their memberships, such as `paid_subscriber`, `canceling`, or `churned`. `null` until it is first computed.

Available options: `canceling`, `churned`, `finished_split_pay`, `paused`, `paid_subscriber`, `paid_once`, `expiring`, `joined`, `left`, `trialing`, `pending_entry`, `renewing`, `past_due`

**most\_recent\_action\_at** `string | null` *required*

When `most_recent_action` took effect, as an ISO 8601 timestamp. `null` when
`most_recent_action` is `null`.

**name** `string | null` *required*

The name the account gave this customer with `PATCH /members/:id`. Invoices
show it as the customer name on every invoice for this member, in place of the
user's own name. `null` when the account has not set one.

**phone\_number** `string | null` *required*

The member's phone number: the most recent number collected (or verified) at
checkout, otherwise their account number when they have shared one with this
seller. `null` without `member:phone:read` on the account, or when there is
neither.

**status** `string` *required*

`joined` while the member is part of the account, `left` after they leave.

Available options: `joined`, `left`

**token\_balance** `number` *required*

The member's current token balance for this account, computed from token
transactions.

**unsubscribe\_status** `string | null` *required*

`unsubscribed` once the member opts out of the account's marketing communications, `null` while they're subscribed. Only the member can change it.

Available options: `unsubscribed`

**usd\_total\_spent** `object` *required*

How much the member has spent on the account's products, in USD.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**user** `object | null` *required*

The user behind this member. `null` when the buyer is another business rather than a person.

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
> **city** `string | null` *required*
>
> The user's city, derived from their IP address. `null` when unknown or the
> user turned off location sharing.
>
> **country** `string | null` *required*
>
> The user's country as an ISO 3166-1 alpha-2 code, like `US`, derived from
> their IP address. `null` when unknown or the user turned off location sharing.
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
> **state** `string | null` *required*
>
> The user's state or region code, like `CA`, derived from their IP address.
> `null` when unknown or the user turned off location sharing.
>
> **username** `string` *required*
>
> Public username.

```json Member theme={null}
{
	"id": "mber_xxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"buyer_account_id": null,
	"access_level": "customer",
	"status": "joined",
	"display_name": "Dana Whitfield",
	"name": "Whitfield Auto Care LLC",
	"email": "dana@shinetime.example",
	"most_recent_action": "paid_subscriber",
	"most_recent_action_at": "2026-07-01T00:00:00.000Z",
	"usd_total_spent": {
		"currency": "usd",
		"amount": "150.00",
		"decimals": 2,
		"display_decimals": 2
	},
	"token_balance": 70.25,
	"earnings_usd": {
		"lifetime": "4120.25",
		"last_30_days": "260.00",
		"last_7_days": "60.00",
		"last_24_hours": "12.50"
	},
	"phone_number": "+12025550123",
	"joined_at": "2026-07-01T00:00:00.000Z",
	"last_accessed_at": "2026-07-20T15:30:00.000Z",
	"last_activity_at": "2026-07-18T09:12:00.000Z",
	"last_activity": {
		"id": "post_xxxxxxxxxxxxx",
		"type": "message",
		"channel_id": "feed_xxxxxxxxxxxxx",
		"author": {
			"id": "user_xxxxxxxxxxxxx",
			"username": "danawhitfield",
			"name": "Dana Whitfield",
			"profile_picture": {
				"url": "https://cdn.whop.com/user.png"
			},
			"badges": [
				{
					"type": "verified_partner"
				}
			]
		},
		"plain_content": "Hey, my access stopped working",
		"created_at": "2026-07-18T09:12:00.000Z"
	},
	"unsubscribe_status": null,
	"metadata": {
		"crm_owner": "jordan",
		"deal_stage": "qualified"
	},
	"created_at": "2026-07-01T00:00:00.000Z",
	"user": {
		"id": "user_xxxxxxxxxxxxx",
		"username": "danawhitfield",
		"name": "Dana Whitfield",
		"profile_picture": {
			"url": "https://cdn.whop.com/user.png"
		},
		"city": "Austin",
		"state": "TX",
		"country": "US",
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	}
}
```


## Related topics

- [Channel Members API](https://docs.whop.com/api-reference/channel-members/channel-member.md)
- [Team Members API](https://docs.whop.com/api-reference/team-members/team-member.md)
- [Member](https://docs.whop.com/api-reference/legacy/members/member.md)
