> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Advertising

> Run ads through Whop with account-level billing. Learn how shared payments, delivery failures, and account payment retries work.

Advertising lets you buy ads on networks like Meta from your Whop account. Whop owns the ad account, the review and launch path, and the billing relationship. You create a campaign through the API instead of connecting an ad account you manage yourself.

## What you can build

* **Run campaigns** on Meta, with an objective, a budget, and a schedule.
* **Target an audience** by location, demographics, interests, devices, and languages, or upload a customer list as an [audience](/api-reference/beta/audiences/audience).
* **Generate creatives** as AI images or video from a prompt, supply your own assets, or promote a post that already exists on a connected social account.
* **Read attributed performance**: spend, results, cost per result, and return on ad spend, all on the campaign object itself.

## Before you spend

Three things gate a live ad, and all three are one-time setup:

* **A Facebook page** for ads to run under. [`GET /social_accounts`](/api-reference/beta/social-accounts/list-social-accounts) must return a `facebook` entry.
* **A payment method** connected in the dashboard, either your Whop balance or a card. Drafts work without one.
* **The Whop pixel** on your destination, unless you send traffic to a whop.com store page. Any external landing page needs [the pixel](/developer/ads/pixel) first.

The [Ads API overview](/api-reference/beta/ads/overview) covers each of these with the exact calls, the required scopes, and the error strings you get when one is missing.

## Core objects

Ad campaign, ad group, ad, audience. Each is defined in [Core Concepts](/developer/concepts).

Advertising has one hierarchy, and every object belongs to the level above it.

<ResponseField name="ad campaign" type="adcamp_">
  Holds the objective, the platform, and the budget strategy. See [Ad Campaigns](/api-reference/beta/ad-campaigns/ad-campaign).
</ResponseField>

<ResponseField name="ad group" type="adgrp_">
  Holds targeting, the optimization goal, the schedule, and its own budget by default. See [Ad Groups](/api-reference/beta/ad-groups/ad-group).
</ResponseField>

<ResponseField name="ad" type="ad_">
  Holds the creatives, the copy variants, the call to action, and the destination URL. See [Ads](/api-reference/beta/ads/ad).
</ResponseField>

[`POST /ads`](/api-reference/beta/ads/create-an-ad) accepts an inline `ad_group`, which accepts an inline `ad_campaign`, so one request creates the whole tree in a single transaction. Pass `ad_group_id` instead to attach the ad to an ad group that already exists.

To launch later rather than now, set `status` to `draft` on the inline `ad_campaign`, then activate it with [`PATCH /ad_campaigns/{id}`](/api-reference/beta/ad-campaigns/update-an-ad-campaign).

Budget lives at exactly one level, never both: on the ad group by default, or on the campaign when you set `budget_optimization` to `ad_campaign`.

## How results are counted

Every performance number Whop reports is attributed by the **Whop pixel**, not by the ad network.

`result_event` names the conversion event a campaign is judged on, and `results` is the pixel-attributed count of it. `cost_per_result` divides spend by that count. When a campaign's ad groups optimize toward different goals there's no single `result_event`, so it's `null` and `results` becomes the sum of each ad group's own attributed results. When nothing Whop-attributable is being optimized for, both are `null` rather than zero.

## Paying for ads

Spend accrues as ads deliver and is charged afterward. **Billing belongs to the account, across its campaigns.** Each collection combines eligible unpaid campaign spend into one payment using the account's configured ads payment methods. You don't configure a separate payment method or request a separate retry for each campaign. The `spend` field on a campaign is the amount charged to you, in `spend_currency`.

Collection timing depends on accrued spend and billing thresholds. Consolidation doesn't mean one payment per day. AI creative generation uses its own balance payment. Too little balance returns a `402` carrying a `deposit_url`.

### When a payment fails

An account payment failure blocks delivery across its billable campaigns. Each affected campaign reports `delivery_status: "payment_failed"`, while its configured `status` stays `active` or `paused`. Read `delivery_status` to determine whether a campaign can deliver, and `issues` for the failure reason.

The account can't activate campaigns while it has unresolved payment failures. You can still pause a campaign to keep it off after payment recovery. Whop also schedules automatic retries. You can request a retry after fixing the payment method.

### Retry a payment

1. In your account's Ads dashboard, select **Update payment method** in the failed-payment banner, or open Ads settings and select **Billing**. Update the account's card or fund the balance it uses.
2. Select **Retry payment** once for the account. API integrations should call [Retry Failed Ads Payments](/api-reference/beta/accounts/retry-failed-ads-payments) with the account's `biz_` ID and `ad_campaign:update` permission.
3. Wait for the background attempt. A `202` response with `queued: true` confirms the job was accepted, **not that the payment succeeded**. Read the account's [ad campaigns](/api-reference/beta/ad-campaigns/list-ad-campaigns) for the outcome.
4. After successful settlement, the payment block clears. Delivery can show `processing` while it's recalculated and synchronized. Campaigns configured as active can resume if their other delivery requirements are met. Campaigns configured as paused stay paused.

```bash theme={null}
curl -X POST "https://api.whop.com/api/v1/accounts/biz_YOUR_ACCOUNT/retry_ads_payment" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-09-22"
```

```json theme={null}
{ "account_id": "biz_YOUR_ACCOUNT", "queued": true }
```

Repeated requests while an account retry is queued or running return an error asking you to wait. If the payment fails again, delivery remains blocked: check the failure reason and fix the payment method before trying again. Don't loop over campaigns to retry each one. The older [campaign retry endpoint](/api-reference/beta/ad-campaigns/retry-a-failed-ad-campaign-payment) still queues an account retry on API versions before `2026-09-22`. Starting with `2026-09-22`, it returns `410 Gone`. Use the account endpoint above.

Older campaigns whose configured `status` was already `payment_failed` return to `paused` after successful retry, because their previous active/paused setting wasn't retained.

## Why a campaign isn't delivering

`delivery_status` answers this directly. It's the first field to read when a campaign exists but nothing is happening. Several states can be true at once, so the value is the highest one on this ladder:

| `delivery_status`  | What it means                                                                                     |
| ------------------ | ------------------------------------------------------------------------------------------------- |
| `payment_failed`   | Account billing failed. Delivery is blocked without changing the configured active/paused setting |
| `all_ads_rejected` | Every ad in the campaign was rejected                                                             |
| `draft`            | Not submitted for delivery                                                                        |
| `no_ad_groups`     | The campaign has no ad groups                                                                     |
| `no_ads`           | The campaign has no ads in any group                                                              |
| `paused`           | You paused the campaign                                                                           |
| `processing`       | The campaign hasn't reached the ad network, or every ad group is still processing                 |
| `issues`           | The campaign has open issues, or every ad has one. Read the `issues` array                        |
| `scheduled`        | The campaign's start is in the future, or every ad group is waiting for its own start             |
| `completed`        | The campaign's end time has passed. A campaign with no end never completes                        |
| `ad_groups_off`    | Every ad group is paused                                                                          |
| `active`           | Delivering                                                                                        |

Ads and ad groups carry their own `delivery_status` with different values. A new ad passes through `in_review` before it delivers.

## Where to go next

<Columns cols={2}>
  <Card title="Create and launch an ad" icon="bullhorn" href="/api-reference/beta/ads/overview">
    The full flow, from creative to live ad, in two calls.
  </Card>

  <Card title="Install the pixel" icon="crosshairs" href="/developer/ads/pixel">
    Required before results can be attributed.
  </Card>

  <Card title="Build an audience" icon="users" href="/api-reference/beta/audiences/create-audience">
    Upload a customer list to target or exclude.
  </Card>

  <Card title="Ads reference" icon="code" href="/api-reference/beta/ad-campaigns/ad-campaign">
    Every advertising endpoint, with a playground.
  </Card>
</Columns>
