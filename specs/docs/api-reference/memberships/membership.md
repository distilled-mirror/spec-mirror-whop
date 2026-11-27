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

# Memberships API

> A Membership is a customer's purchase of a variant: the subscription or one-time grant that gives them access to a product.

It tracks billing state (`active`, `trialing`, `past_due`, and so on), the current period, pending cancellations, custom metadata, and the software license key when the product includes licensing.

Use the Memberships API to list an account's memberships or your own, retrieve one by ID or license key, invite a recipient to join through a free variant, and manage the lifecycle: cancel immediately or at period end, reverse a scheduled cancellation, pause and resume payment collection, extend or reactivate access, apply a promo code or affiliate, change renewal cadence, resync access, generate a transfer link, and update metadata.

> [!NOTE]
>
> Replaces the Legacy [Memberships](https://docs.whop.com/api-reference/memberships/membership)
> resource. Existing Legacy integrations keep working; see [API
> versions](https://docs.whop.com/developer/api/versioning.md) for the stability contract.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Memberships](https://docs.whop.com/api-reference/memberships/list-memberships.md) | GET `/memberships` |
| [Retrieve Membership](https://docs.whop.com/api-reference/memberships/retrieve-membership.md) | GET `/memberships/{id}` |
| [Apply Promo Code to Membership](https://docs.whop.com/api-reference/memberships/apply-promo-code-to-membership.md) | POST `/memberships/{id}/apply_promo_code` |
| [Assign Affiliate to Membership](https://docs.whop.com/api-reference/memberships/assign-affiliate-to-membership.md) | POST `/memberships/{id}/assign_affiliate` |
| [Cancel Membership](https://docs.whop.com/api-reference/memberships/cancel-membership.md) | POST `/memberships/{id}/cancel` |
| [Extend Membership](https://docs.whop.com/api-reference/memberships/extend-membership.md) | POST `/memberships/{id}/extend` |
| [Pause Membership](https://docs.whop.com/api-reference/memberships/pause-membership.md) | POST `/memberships/{id}/pause` |
| [Reactivate Membership](https://docs.whop.com/api-reference/memberships/reactivate-membership.md) | POST `/memberships/{id}/reactivate` |
| [Resume Membership](https://docs.whop.com/api-reference/memberships/resume-membership.md) | POST `/memberships/{id}/resume` |
| [Resync Membership Access](https://docs.whop.com/api-reference/memberships/resync-membership-access.md) | POST `/memberships/{id}/resync_access` |
| [Transfer Membership](https://docs.whop.com/api-reference/memberships/transfer-membership.md) | POST `/memberships/{id}/transfer` |
| [Invite to a Membership](https://docs.whop.com/api-reference/memberships/invite-to-a-membership.md) | POST `/memberships/invite` |
| [Update Membership](https://docs.whop.com/api-reference/memberships/update-membership.md) | PATCH `/memberships/{id}` |

## Attributes

**id** `string` *required*

Membership ID, prefixed `mem_`.

**account** `object` *required*

The account (seller) this membership belongs to.

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

**affiliate** `object | null` *required*

The membership's affiliate commission. `null` without an affiliate, without `affiliate:basic:read` on the account, and always in webhooks.

> [accordion] Properties
>
> **id** `string` *required*
>
> Affiliate ID, prefixed `aff_`.
>
> **applies\_to\_payments** `string` *required*
>
> Whether the commission is paid on the membership's first payment only or on all its payments.
>
> Available options: `first_payment`, `all_payments`
>
> **commission\_amount** `object | null` *required*
>
> The flat fee commission in the membership currency, or `null` for a percentage.
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
> **commission\_percentage** `number | null` *required*
>
> The percentage commission in percentage points, so `20` means 20%, or `null`
> for a flat fee.
>
> **commission\_source** `string` *required*
>
> How the commission rate was set: `custom` for a rate set for this affiliate, `member` for the member affiliate rate of the plan or product, `global` for the product's global affiliate rate, or `marketplace` for a Whop marketplace referral.
>
> Available options: `custom`, `member`, `marketplace`, `global`
>
> **commission\_type** `string` *required*
>
> Whether the commission is a percentage or a flat fee.
>
> Available options: `flat_fee`, `percentage`
>
> **enabled** `boolean` *required*
>
> Whether this membership pays affiliate commissions. `false` means the
> affiliate is kept on the membership but is no longer paid.
>
> **user\_id** `string` *required*
>
> ID of the affiliate's user, prefixed `user_`.

**billing\_period\_days** `integer | null` *required*

Number of days between recurring charges. `null` for non-renewing memberships
or memberships with multiple renewal schedules.

**cancel\_at\_period\_end** `boolean` *required*

Whether the membership is set to cancel when the current billing period ends.
Only meaningful for recurring variants.

**cancel\_option** `string | null` *required*

The category the member picked for why they canceled. `null` when no category was picked.

Available options: `too_expensive`, `switching`, `missing_features`, `technical_issues`, `bad_experience`, `unused`, `other`, `testing`

**canceled\_at** `string | null` *required*

When cancellation was requested, or when the membership was canceled if no
request time is recorded, as an ISO 8601 timestamp. `null` when neither is
recorded.

**cancellation\_reason** `string | null` *required*

Free-text explanation provided when canceling. `null` when no reason was
provided.

**collection\_method** `string | null` *required*

How renewals are collected. `charge_automatically` charges the saved payment method each period; `send_invoice` emails the buyer an invoice each period. `null` when not set.

Available options: `charge_automatically`, `send_invoice`

**created\_at** `string` *required*

When the membership was created, as an ISO 8601 timestamp.

**current\_period\_end** `string | null` *required*

When the current billing period renews, or when a non-renewing membership
expires, as an ISO 8601 timestamp. `null` for one-time purchases with no
expiration.

**current\_period\_start** `string | null` *required*

When the current billing period started, as an ISO 8601 timestamp. `null` when
no billing period is recorded.

**expires\_at** `string | null` *required*

When access ends for a membership with a fixed expiration, as an ISO 8601
timestamp. `null` when the membership has no expiration, such as a renewing
membership.

**external\_accounts** `object[] | null` *required*

The member's accounts on outside platforms, such as Discord and Telegram. Empty when the member has none linked. `null` in webhooks.

> [accordion] Properties
>
> **external\_id** `string` *required*
>
> The member's user ID on the platform, such as a Discord user ID. Prefixed with the platform (`discord_\<external_id>`), it is the `external_account` value List Memberships filters by.
>
> **platform** `string` *required*
>
> The platform the member's account is on.
>
> Available options: `discord`, `telegram`
>
> **username** `string | null` *required*
>
> The member's username on the platform. `null` when Whop doesn't know it, such as for a membership imported with only a Discord ID.

**feedback\_option** `object | null` *required*

The account's own cancel reason the member picked, as they saw it. `null` when the member picked a default category or gave no reason.

> [accordion] Properties
>
> **label** `string` *required*
>
> The option's label when the member picked it. Renaming the option later does not change it.

**initial\_price** `object` *required*

The variant's current up-front price, in the variant's currency. A list price, not necessarily what the buyer paid. `amount` is `"0.00"` when the first charge is free.

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

**license\_key** `string | null` *required*

The software license key for this membership. Only present when the product
includes a software licensing experience.

**manage\_url** `string | null` *required*

URL where the buyer can sign in to manage billing. `null` without a member
record or unless the caller is the buyer or has `member:manage` on the
account.

**member** `object | null` *required*

The caller's member row on the account. Present only when the membership belongs to the caller; `null` on seller-side reads.

> [accordion] Properties
>
> **access\_level** `string` *required*
>
> What the member can reach on the account: `customer` for paying members, `admin` for team members, `no_access` once every grant has lapsed.
>
> Available options: `no_access`, `admin`, `customer`
>
> **last\_accessed\_at** `string | null` *required*
>
> When the member last opened the account's content, as an ISO 8601 timestamp.
> `null` if they never have.
>
> **position** `number | null` *required*
>
> The member's sort position in the buyer's own account list. `null` until they arrange it.

**member\_id** `string | null` *required*

The buyer's member record on the account, prefixed `mber_`. `null` when the
membership has no member record, such as an unclaimed membership.

**metadata** `object` *required*

Custom key-value pairs stored on the membership, commonly used for software
licensing.

**most\_recent\_action** `string | null` *required*

The membership's latest lifecycle event, such as `renewing_on` or `canceled_on`. Pair it with `most_recent_action_at`. `null` when none is recorded.

Available options: `canceled_on`, `finished_split_pay_at`, `canceling_on`, `ending_on`, `renewing_on`, `past_due`, `next_payment_attempt`, `paused_at`, `trial_ends_at`, `unresolved_at`, `trial_canceling_on`

**most\_recent\_action\_at** `string | null` *required*

When `most_recent_action` happened or is scheduled to happen, as an ISO 8601
timestamp. A future time for upcoming events such as `renewing_on`. `null`
when no action is recorded.

**phone\_number** `string | null` *required*

The buyer's phone number recorded for this membership, or `null`. The number
collected (or verified) at checkout when the seller's phone collection is on;
falls back to the buyer's account number when they have shared one with this
seller.

**product** `object` *required*

The product this membership grants access to.

> [accordion] Properties
>
> **id** `string` *required*
>
> Product ID, prefixed `prod_`.
>
> **logo\_url** `string | null` *required*
>
> Product logo image URL. `null` when the product has no logo.
>
> **title** `string` *required*
>
> Product display name.

**product\_id** `string` *required*

The product this membership grants access to, prefixed `prod_`.

**promo\_code\_id** `string | null` *required*

The promo code discounting this membership, prefixed `promo_`. `null` when
none is applied. Set at checkout or by Apply Promo Code to Membership.

**renewal\_price** `object | null` *required*

The variant's renewal price, in the variant's currency, after any promo code that still applies. A membership billed in the buyer's local currency is charged this price converted at the exchange rate on each billing date. `null` for non-renewing memberships.

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

**split\_pay\_current\_payments** `integer | null` *required*

Paid installments counted so far on a split pay membership. `null` when the
membership is not on split pay, or before its first installment is counted.

**split\_pay\_required\_payments** `integer | null` *required*

Installment payments required before the membership stops billing. `null` when
the membership is not on split pay.

**status** `string` *required*

Billing state of the membership. `active` and `trialing` memberships grant access. `past_due` is the grace period after a failed payment. `completed` one-time purchases keep access. `scheduled` memberships wait for their variant's start date, when the first charge runs and access starts. `canceled` and `expired` memberships do not grant access.

Available options: `trialing`, `active`, `past_due`, `completed`, `canceled`, `expired`, `unresolved`, `scheduled`

**updated\_at** `string` *required*

When the membership was last changed, as an ISO 8601 timestamp. Reflects the
most recent change to the membership itself, so you can reconcile against
webhook retries, replays, and backfills.

**user\_id** `string | null` *required*

The buyer, prefixed `user_`. `null` when the buyer is another business or the
membership is unclaimed.

**variant\_id** `string` *required*

The variant the buyer purchased, prefixed `plan_`.

```json Membership theme={null}
{
	"id": "mem_xxxxxxxxxxxxxx",
	"affiliate": null,
	"status": "active",
	"collection_method": "charge_automatically",
	"most_recent_action": "renewing_on",
	"most_recent_action_at": "2026-08-01T00:00:00.000Z",
	"user_id": "user_xxxxxxxxxxxxx",
	"product_id": "prod_xxxxxxxxxxxxx",
	"variant_id": "plan_xxxxxxxxxxxxx",
	"member_id": "mber_xxxxxxxxxxxxx",
	"product": {
		"id": "prod_xxxxxxxxxxxxx",
		"title": "Unlimited Wash Club",
		"logo_url": "https://cdn.whop.com/product.png"
	},
	"initial_price": {
		"currency": "usd",
		"amount": "0.00",
		"decimals": 2,
		"display_decimals": 2
	},
	"renewal_price": {
		"currency": "usd",
		"amount": "50.00",
		"decimals": 2,
		"display_decimals": 2
	},
	"split_pay_current_payments": null,
	"split_pay_required_payments": null,
	"expires_at": null,
	"promo_code_id": null,
	"billing_period_days": 30,
	"license_key": "A1B2C3-D4E5F6-G7H8I9",
	"phone_number": "+12025550123",
	"cancel_at_period_end": false,
	"current_period_start": "2026-07-01T00:00:00.000Z",
	"current_period_end": "2026-08-01T00:00:00.000Z",
	"canceled_at": null,
	"cancel_option": null,
	"cancellation_reason": null,
	"feedback_option": null,
	"external_accounts": [
		{
			"platform": "discord",
			"external_id": "123456789012345678",
			"username": "danawhitfield"
		}
	],
	"manage_url": "https://whop.com/billing/manage/mber_xxxxxxxxxxxxx",
	"metadata": {
		"external_customer_id": "cus_123"
	},
	"created_at": "2026-07-01T00:00:00.000Z",
	"updated_at": "2026-07-15T00:00:00.000Z",
	"member": {
		"access_level": "customer",
		"last_accessed_at": "2026-07-20T15:30:00.000Z",
		"position": 1
	},
	"account": {
		"id": "biz_xxxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing",
		"route": "shine-time-auto-detailing",
		"logo_url": "https://cdn.whop.com/account.png",
		"banner_image_url": "https://cdn.whop.com/account-banner.png"
	}
}
```


## Related topics

- [Memberships](https://docs.whop.com/developer/guides/memberships.md)
- [Membership](https://docs.whop.com/api-reference/legacy/memberships/membership.md)
- [Cancel membership](https://docs.whop.com/api-reference/legacy/memberships/cancel-membership.md)
