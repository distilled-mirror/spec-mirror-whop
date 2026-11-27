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

# Partner Referral Requests API

> A Partner Referral Request is a partner's referral link, or a request to be credited as the referrer of an existing business.

An attribution request takes effect only when an owner of the business accepts it.

Use the Partner Referral Requests API to create referral links and send attribution requests. You can also list the links and requests you've sent or received, and accept, decline, or cancel a pending request.

## Endpoints

| Endpoint | Request |
| - | - |
| [List partner referral requests](https://docs.whop.com/api-reference/partner-referral-requests/list-partner-referral-requests.md) | GET `/partner_referral_requests` |
| [Retrieve a partner referral request](https://docs.whop.com/api-reference/partner-referral-requests/retrieve-a-partner-referral-request.md) | GET `/partner_referral_requests/{id}` |
| [Create a partner referral request](https://docs.whop.com/api-reference/partner-referral-requests/create-a-partner-referral-request.md) | POST `/partner_referral_requests` |
| [Accept a partner referral request](https://docs.whop.com/api-reference/partner-referral-requests/accept-a-partner-referral-request.md) | POST `/partner_referral_requests/{id}/accept` |
| [Cancel a partner referral request](https://docs.whop.com/api-reference/partner-referral-requests/cancel-a-partner-referral-request.md) | POST `/partner_referral_requests/{id}/cancel` |
| [Decline a partner referral request](https://docs.whop.com/api-reference/partner-referral-requests/decline-a-partner-referral-request.md) | POST `/partner_referral_requests/{id}/decline` |

## Attributes

**id** `string` *required*

Partner referral request ID, prefixed `prfr_`.

**account** `object | null` *required*

Business receiving the request, when one is assigned.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **title** `string` *required*
>
> Account display name.

**authorized\_user\_id** `string | null` *required*

The partner's team membership this referral was created for, prefixed `ausr_`.
`null` when the referral is personal.

**code** `string | null` *required*

Unique referral code, when assigned.

**created\_at** `string` *required*

When the request was created, as an ISO 8601 timestamp.

**expires\_at** `string | null` *required*

When a lead offer stops taking a response, as an ISO 8601 timestamp. `null`
for other request types.

**lead** `object | null` *required*

The business's signup answers on a lead offer. Only the partner it was offered to sees them. `null` for anyone else and for other request types.

> [accordion] Properties
>
> **business\_type** `string | null` *required*
>
> The business type Whop classified the business into, the same value as the
> account's `business_type`, such as `education`. `null` until the business is
> classified.
>
> **country** `string | null` *required*
>
> The business's country as a two-letter ISO 3166-1 code, such as `us`.
>
> **industry\_type** `object | null` *required*
>
> The industry type Whop classified the business into, the same one the account's `industry_type` names. `null` until the business is classified, or when no industry type fits.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Industry type slug, such as `equity_research`. With `business_type`, it
> > identifies one industry type.
> >
> > **business\_type** `object` *required*
> >
> > The business type the industry type belongs to.
> >
> > > [accordion] Properties
> > >
> > > **id** `string` *required*
> > >
> > > Business type slug, such as `education`.
> > >
> > > **name** `string` *required*
> > >
> > > Name of the business type in the viewer's language, such as `Education`.
> >
> > **name** `string` *required*
> >
> > Name of the industry type in the viewer's language, such as `Equity research`.
>
> **migrating\_from** `string | null` *required*
>
> The platform the owner said they are moving from, such as `stripe`, or `none`.
> `null` when the form did not ask.
>
> **owner** `object` *required*
>
> The business owner. A partner messages them with this user's ID.
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
> **owner\_email** `string | null` *required*
>
> The business owner's email address. `null` until the partner accepts the
> offer.
>
> **owner\_phone** `string | null` *required*
>
> The business owner's phone number in E.164 format, such as `+14155552671`.
> `null` until the partner accepts the offer, or when the owner gave none.
>
> **revenue\_band** `string | null` *required*
>
> The revenue range the owner picked at signup, such as `0-50k` or `50-250k`.
> `null` when they skipped it.
>
> **signed\_up\_at** `string` *required*
>
> When the business was created, as an ISO 8601 timestamp.
>
> **website** `string | null` *required*
>
> The business's website. `null` when the business has none.

**max\_redemptions** `integer | null` *required*

Maximum permitted redemptions, when limited.

**member\_id** `string | null` *required*

The partner's member record this referral was created for, prefixed `mber_`,
when their role on that business is held on their member record. `null`
otherwise.

**partner** `object` *required*

Public profile of the partner associated with this referral request.

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

**referring\_account** `object | null` *required*

The partner's team account that this referral counts toward. `null` for a personal referral. A lead offer counts toward the partner-mode team where the partner earned the most in the last 30 days, or the newest on a tie. It is set when the offer is made.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **title** `string` *required*
>
> Account display name.

**request\_type** `string` *required*

How the referral request was initiated.

Available options: `manual`, `ownership_transfer`, `link`, `lead`

**rewards** `object[]` *required*

Rewards offered by this request.

> [accordion] Properties
>
> **id** `string` *required*
>
> Partner reward ID, prefixed `prwd_`.
>
> **bot\_qualification\_type** `string | null` *required*
>
> Activity that qualifies for this reward, when specified.
>
> Available options: `sales`, `ad_spend`
>
> **qualification\_amount** `object` *required*
>
> USD amount of qualifying activity required to earn the reward.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **recipient** `string | null` *required*
>
> Reward recipient's role, or null for a fixed user who is not the requesting partner.
>
> Available options: `business`, `partner`
>
> **recipient\_id** `string | null` *required*
>
> Concrete recipient's user or business ID, or null until a business redeems the
> offer.
>
> **reward\_amount** `object` *required*
>
> USD amount granted after qualification.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**status** `string | null` *required*

The approval state, or `null` for requests without an approval process. A lead offer that gets no answer before `expires_at` becomes `cancelled`.

Available options: `pending`, `accepted`, `denied`, `cancelled`

**status\_reason** `string | null` *required*

Why the request reached its status. `expired` means a lead offer got no answer before `expires_at`. `passed` means the partner turned down a lead offer. `withdrawn` means the partner cancelled their own request. `null` for any other status.

Available options: `expired`, `passed`, `withdrawn`

**updated\_at** `string` *required*

When the request last changed, as an ISO 8601 timestamp.

**user** `object | null` *required*

The user an attribution request was sent to. Deprecated: requests to a user have ended. Always `null`. DEPRECATED: No longer populated. Always null.

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

```json PartnerReferralRequest theme={null}
{
	"id": "prfr_xxxxxxxxxxxxxx",
	"request_type": "manual",
	"status": "accepted",
	"status_reason": null,
	"code": null,
	"max_redemptions": null,
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing"
	},
	"referring_account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Whitfield Growth Partners"
	},
	"partner": {
		"id": "user_xxxxxxxxxxxxxx",
		"name": "Dana Whitfield",
		"username": "danawhitfield",
		"profile_picture": {
			"url": "https://ui-avatars.com/api/?name=Dana+Whitfield"
		},
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"user": null,
	"lead": null,
	"rewards": [
		{
			"id": "prwd_xxxxxxxxxxxxxx",
			"recipient": "partner",
			"recipient_id": "user_xxxxxxxxxxxxxx",
			"bot_qualification_type": "sales",
			"qualification_amount": {
				"amount": "1000.00",
				"currency": "usd",
				"decimals": 2,
				"display_decimals": 2
			},
			"reward_amount": {
				"amount": "100.00",
				"currency": "usd",
				"decimals": 2,
				"display_decimals": 2
			}
		}
	],
	"created_at": "2026-09-10T09:00:00.000Z",
	"updated_at": "2026-09-11T17:20:00.000Z",
	"authorized_user_id": null,
	"member_id": null,
	"expires_at": null
}
```


## Related topics

- [Create a Partner Referral Request](https://docs.whop.com/api-reference/partner-referral-requests/create-a-partner-referral-request.md)
- [Retrieve a Partner Referral Request](https://docs.whop.com/api-reference/partner-referral-requests/retrieve-a-partner-referral-request.md)
- [List Partner Referral Requests](https://docs.whop.com/api-reference/partner-referral-requests/list-partner-referral-requests.md)
