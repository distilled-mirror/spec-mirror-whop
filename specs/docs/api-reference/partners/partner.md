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

# Partners API

> Get started at whop.com/network. A Partner is a user who refers people and businesses to Whop.

Get started at [whop.com/network](https://whop.com/network). A Partner is a user who refers people and businesses to Whop. The partner profile includes enrollment, active direct business referral counts, and default payout terms.

Retrieve your profile with `/partners/me`. Use `/partner_referral_requests` to create and manage referral links and their rewards. You can also enroll in the partner program, review referred users and businesses, track earnings, and see the partner leaderboard.

## Endpoints

| Endpoint | Request |
| - | - |
| [Retrieve a partner](https://docs.whop.com/api-reference/partners/retrieve-a-partner.md) | GET `/partners/{id}` |
| [List referred businesses](https://docs.whop.com/api-reference/partners/list-referred-businesses.md) | GET `/partners/businesses` |
| [Retrieve a referred business](https://docs.whop.com/api-reference/partners/retrieve-a-referred-business.md) | GET `/partners/businesses/{id}` |
| [List referred business earnings](https://docs.whop.com/api-reference/partners/list-referred-business-earnings.md) | GET `/partners/businesses/{id}/earnings` |
| [Retrieve the fee terms a partner can offer](https://docs.whop.com/api-reference/partners/retrieve-the-fee-terms-a-partner-can-offer.md) | GET `/partners/fee_options` |
| [Retrieve the leaderboard](https://docs.whop.com/api-reference/partners/retrieve-the-leaderboard.md) | GET `/partners/leaderboard` |
| [Enroll as a Whop partner](https://docs.whop.com/api-reference/partners/enroll-as-a-whop-partner.md) | POST `/partners` |
| [Accept a CPA offer](https://docs.whop.com/api-reference/partners/accept-a-cpa-offer.md) | POST `/partners/businesses/{id}/accept_commission_advance` |
| [Resend an owner invite](https://docs.whop.com/api-reference/partners/resend-an-owner-invite.md) | POST `/partners/businesses/{id}/resend_invite` |
| [Save CPA projections](https://docs.whop.com/api-reference/partners/save-cpa-projections.md) | PATCH `/partners/businesses/{id}` |

## Attributes

**account\_id** `string | null` *required*

The business whose `payout_rates` these are, prefixed `biz_`, when the request
sent `account_id`. `null` when the rates are the caller's own.

**certification\_complete** `boolean` *required*

Whether the user finished the partner certification course: every visible quiz
and knowledge check has a passing result, or, when the course has none, every
visible lesson is marked completed.

**joined\_at** `string | null` *required*

When the user joined the partner program, as an ISO 8601 timestamp. Null when
they have not joined.

**lead\_slot\_limit** `integer` *required*

How many leads the partner can hold at once. A waiting lead offer takes a
slot, and so does a lead accepted in the last 30 days whose business has made
\$1 or less.

**lead\_slots\_used** `integer` *required*

How many of the partner's lead slots are taken now.

**payout\_rates** `object[]` *required*

Default payout percentages grouped by referral tier, with one earning duration per tier. The business's when the request sent `account_id`, else the caller's own. Existing businesses can have individual terms.

> [accordion] Properties
>
> **duration** `object` *required*
>
> Default period during which a new referred business can generate commissions, measured from its attribution start. This is not a payout delay. Individual business terms can differ.
>
> > [accordion] Properties
> >
> > **unit** `string` *required*
> >
> > Unit of the earning window. Month means a calendar month; day means a day.
> >
> > Available options: `day`, `month`
> >
> > **value** `integer` *required*
> >
> > Number of units in the earning window.
>
> **rates** `object[]` *required*
>
> Default payout percentages for each income source in this referral tier.
>
> > [accordion] Properties
> >
> > **income\_source** `string` *required*
> >
> > Income source that generates this percentage payout.
> >
> > Available options: `sales`, `transfer`, `card_interchange`, `ad_spend`, `withdrawal`
> >
> > **percentage** `number` *required*
> >
> > Partner's default percentage for this tier and income source. For example, 30 means 30%.
>
> **tier** `string` *required*
>
> Referral tier: first for a directly referred business, or second for a business brought by a referred partner.
>
> Available options: `first`, `second`

**referred\_businesses\_count** `integer` *required*

Number of active first-tier business referrals attributed to the partner,
excluding deleted businesses.

**referring\_partner** `object | null` *required*

The user who referred this user. Deprecated: no longer populated. Always `null`. DEPRECATED: No longer populated. Always null.

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

**user** `object` *required*

The authenticated partner's public profile.

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

**verification\_waitlist\_joined** `boolean` *required*

Whether the user has a pending or approved personal entry on the Verified
Partner waitlist.

**whop\_partner\_verified\_at** `string | null` *required*

When the user became a verified Whop Partner, as an ISO 8601 timestamp. `null`
if not verified.

```json Partner theme={null}
{
	"user": {
		"id": "user_9mHw2b4TkL8pQ",
		"username": "danawhitfield",
		"name": "Dana Whitfield",
		"profile_picture": {
			"url": "https://shinetime.example/dana-avatar.png"
		},
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"joined_at": "2026-09-01T12:00:00.000Z",
	"whop_partner_verified_at": null,
	"verification_waitlist_joined": false,
	"certification_complete": false,
	"referred_businesses_count": 12,
	"lead_slot_limit": 5,
	"lead_slots_used": 3,
	"referring_partner": null,
	"account_id": null,
	"payout_rates": [
		{
			"tier": "first",
			"duration": {
				"unit": "month",
				"value": 600
			},
			"rates": [
				{
					"income_source": "sales",
					"percentage": 30
				},
				{
					"income_source": "transfer",
					"percentage": 30
				},
				{
					"income_source": "card_interchange",
					"percentage": 30
				},
				{
					"income_source": "ad_spend",
					"percentage": 1
				}
			]
		},
		{
			"tier": "second",
			"duration": {
				"unit": "month",
				"value": 600
			},
			"rates": [
				{
					"income_source": "sales",
					"percentage": 10
				},
				{
					"income_source": "transfer",
					"percentage": 10
				},
				{
					"income_source": "card_interchange",
					"percentage": 10
				},
				{
					"income_source": "ad_spend",
					"percentage": 0.5
				}
			]
		}
	]
}
```


## Related topics

- [Whop Partners API](https://docs.whop.com/developer/partners/overview.md)
- [Partner Referral Requests API](https://docs.whop.com/api-reference/partner-referral-requests/partner-referral-request.md)
- [Retrieve a Partner](https://docs.whop.com/api-reference/partners/retrieve-a-partner.md)
