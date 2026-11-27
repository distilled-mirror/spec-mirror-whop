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

# Waitlist Entries API

> A Waitlist Entry is a user's signup for a variant that requires seller approval.

Joining doesn't grant a membership or charge a payment method; the seller approves or denies each signup.

Use the Waitlist Entries API to join a waitlist, read or cancel your own signups, and approve or deny the signups for accounts you operate.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Waitlist Entries](https://docs.whop.com/api-reference/waitlist-entries/list-waitlist-entries.md) | GET `/waitlist_entries` |
| [Retrieve Waitlist Entry](https://docs.whop.com/api-reference/waitlist-entries/retrieve-waitlist-entry.md) | GET `/waitlist_entries/{id}` |
| [Create Waitlist Entry](https://docs.whop.com/api-reference/waitlist-entries/create-waitlist-entry.md) | POST `/waitlist_entries` |
| [Approve Waitlist Entry](https://docs.whop.com/api-reference/waitlist-entries/approve-waitlist-entry.md) | POST `/waitlist_entries/{id}/approve` |
| [Cancel Waitlist Entry](https://docs.whop.com/api-reference/waitlist-entries/cancel-waitlist-entry.md) | POST `/waitlist_entries/{id}/cancel` |
| [Deny Waitlist Entry](https://docs.whop.com/api-reference/waitlist-entries/deny-waitlist-entry.md) | POST `/waitlist_entries/{id}/deny` |
| [Approve All Waitlist Entries](https://docs.whop.com/api-reference/waitlist-entries/approve-all-waitlist-entries.md) | POST `/waitlist_entries/approve_all` |

## Attributes

**id** `string` *required*

The waitlist signup ID, prefixed `entry_`.

**account\_id** `string` *required*

The seller account, prefixed `biz_`.

**approval\_failure\_reason** `string | null` *required*

Why the last approval attempt failed, or `null` when none has. `plan_unavailable` — the variant, product, or seller account was deleted. `already_member` — the user already has a membership on a one-per-user product. `checkout_failed` — checkout failed, usually a declined payment, and the signup was denied. `unknown` — another failure; retry. Cleared when approval is requeued.

Available options: `plan_unavailable`, `already_member`, `checkout_failed`, `unknown`

**buyer\_account** `object | null` *required*

The account the signup was submitted on behalf of, or `null` when the user signed up for themselves.

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

**buyer\_account\_id** `string | null` *required*

The account the signup was submitted on behalf of, prefixed `biz_`, or `null`
when the user signed up for themselves.

**created\_at** `string` *required*

When this signup was submitted, as an ISO 8601 timestamp.

**custom\_field\_responses** `object[]` *required*

The buyer's answers to checkout questions.

> [accordion] Properties
>
> **id** `string` *required*
>
> The answer's ID, prefixed `cfrp_`.
>
> **answer** `string` *required*
>
> The buyer's answer.
>
> **question** `string` *required*
>
> The question shown when the signup was submitted.

**membership\_id** `string | null` *required*

The membership created for this signup, prefixed `mem_`, or `null` when there
is none. Check the membership's status to determine access.

**metadata** `object` *required*

Custom key-value metadata associated with this signup.

**product** `object | null` *required*

The product this signup belongs to, or `null` when the variant has no product.

> [accordion] Properties
>
> **id** `string` *required*
>
> Product ID, prefixed `prod_`.
>
> **title** `string` *required*
>
> Product display name.

**product\_id** `string | null` *required*

The product this signup belongs to, prefixed `prod_`, or `null` when the
variant has no product.

**status** `string` *required*

The signup's current state. Approval runs asynchronously, so a signup stays `pending` until processing completes. `approved` alone does not prove an active membership; check `membership_id`.

Available options: `pending`, `approved`, `denied`, `canceled`

**updated\_at** `string` *required*

When this signup last changed, as an ISO 8601 timestamp.

**user** `object` *required*

The user who submitted this signup. For a signup an account's own credential submitted, the account's owner.

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

**user\_id** `string` *required*

The user who submitted this signup, prefixed `user_`. For a signup an
account's own credential submitted, the account's owner.

**variant** `object` *required*

The variant this signup belongs to.

> [accordion] Properties
>
> **id** `string` *required*
>
> Variant ID, prefixed `plan_`.
>
> **billing\_period** `number | null` *required*
>
> Number of days between recurring charges, such as 30 for monthly or 365 for
> annual. `null` for one-time variants.
>
> **billing\_type** `string` *required*
>
> Billing model for this variant: `one_time` or `renewal`.
>
> Available options: `renewal`, `one_time`
>
> **description** `string | null` *required*
>
> Customer-visible variant description. Maximum 1000 characters. `null` if no
> description is set.
>
> **initial\_price** `object` *required*
>
> What checkout charges up front. `amount` is `"0.00"` when the first charge is free, such as a trial.
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
> **renewal\_price** `object` *required*
>
> The recurring charge every `billing_period` days. `amount` is `"0.00"` for one-time variants.
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
> **title** `string | null` *required*
>
> Variant display name shown to customers. `null` if no title has been set.
>
> **trial\_period\_days** `number | null` *required*
>
> Free trial days configured before the first renewal charge. `null` if no trial is configured.

**variant\_id** `string` *required*

The variant this signup belongs to, prefixed `plan_`.

```json WaitlistEntry theme={null}
{
	"id": "entry_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"product_id": "prod_xxxxxxxxxxxxxx",
	"product": {
		"id": "prod_xxxxxxxxxxxxxx",
		"title": "Unlimited Wash Club"
	},
	"variant_id": "plan_xxxxxxxxxxxxxx",
	"variant": {
		"id": "plan_xxxxxxxxxxxxxx",
		"title": "Monthly",
		"description": "Billed monthly. Cancel anytime.",
		"billing_type": "renewal",
		"billing_period": 30,
		"initial_price": {
			"currency": "usd",
			"amount": "10.00",
			"decimals": 2,
			"display_decimals": 2
		},
		"renewal_price": {
			"currency": "usd",
			"amount": "10.00",
			"decimals": 2,
			"display_decimals": 2
		},
		"trial_period_days": 7
	},
	"user_id": "user_xxxxxxxxxxxxxx",
	"user": {
		"id": "user_xxxxxxxxxxxxxx",
		"username": "danawhitfield",
		"name": "Dana Whitfield",
		"profile_picture": {
			"url": "https://ui-avatars.com/api/?name=Dana+Whitfield"
		},
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"buyer_account_id": null,
	"buyer_account": null,
	"status": "approved",
	"membership_id": "mem_xxxxxxxxxxxxxx",
	"approval_failure_reason": null,
	"custom_field_responses": [
		{
			"id": "cufi_xxxxxxxxxxxxxx",
			"question": "What car do you drive?",
			"answer": "2021 Tesla Model 3"
		}
	],
	"metadata": {},
	"created_at": "2026-09-12T10:15:00.000Z",
	"updated_at": "2026-09-13T08:02:44.000Z"
}
```


## Related topics

- [Cancel Waitlist Entry](https://docs.whop.com/api-reference/waitlist-entries/cancel-waitlist-entry.md)
- [List Waitlist Entries](https://docs.whop.com/api-reference/waitlist-entries/list-waitlist-entries.md)
- [Retrieve Waitlist Entry](https://docs.whop.com/api-reference/waitlist-entries/retrieve-waitlist-entry.md)
