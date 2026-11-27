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

# Advertise with Whop Ads

> Buy ads on external ad networks through your Whop account, with billing handled at the account level.

Advertising lets you buy ads on external ad networks from your Whop account. Whop owns the ad account, the review and launch path, and the billing relationship. You create a campaign through the API instead of connecting an ad account you manage yourself.

## What you can build

* **Run campaigns** with an objective, a budget, and a schedule.
* **Target an audience** by location, demographics, interests, devices, and languages, or upload a customer list as an [audience](https://docs.whop.com/api-reference/audiences/audience).
* **Generate creatives** as AI images or video from a prompt, supply your own assets, or promote a post that already exists on a connected external account.
* **Read attributed performance**: spend, results, cost per result, and return on ad spend, all on the campaign object itself.

## Countries you cannot target

Whop doesn't allow campaigns to target these countries:

| Country | Country code |
| - | - |
| Sudan | `SD` |
| Russia | `RU` |
| Cuba | `CU` |
| Iran | `IR` |
| North Korea | `KP` |

Requests to create an ad group or update its targeted countries are rejected if they include any of these codes. Remove them from your targeted locations before submitting the request.

## Before you spend

Two things gate a live ad, and both are one-time setup:

* **A payment method** connected in the dashboard, either your Whop balance or a card. Drafts work without one.
* **The Whop pixel** on your destination, unless you send traffic to a whop.com store page. Any external landing page needs [the pixel](https://docs.whop.com/developer/ads/pixel.md) first.

The [Ads API overview](https://docs.whop.com/api-reference/ads/overview.md) covers each of these with the exact calls, the required scopes, and the error strings you get when one is missing.

## Core objects

Ad campaign, ad group, ad, audience. Each is defined in [Core Concepts](https://docs.whop.com/developer/concepts.md).

Advertising has one hierarchy, and every object belongs to the level above it.

**ad campaign** `adcamp_`

Holds the objective and the budget strategy. See [Ad Campaigns](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign).

**ad group** `adgrp_`

Holds targeting, the optimization goal, the schedule, and its own budget by default. See [Ad Groups](https://docs.whop.com/api-reference/ad-groups/ad-group).

**ad** `ad_`

Holds the creatives, the copy variants, the call to action, and the destination URL. See [Ads](https://docs.whop.com/api-reference/ads/ad).

[`POST /ads`](https://docs.whop.com/api-reference/ads/create-an-ad) accepts an inline `ad_group`, which accepts an inline `ad_campaign`, so one request creates the whole tree in a single transaction. Pass `ad_group_id` instead to attach the ad to an ad group that already exists.

To launch later rather than now, set `status` to `draft` on the inline `ad_campaign`, then activate it with [`PATCH /ad_campaigns/{id}`](https://docs.whop.com/api-reference/ad-campaigns/update-an-ad-campaign).

Budget lives at exactly one level, never both: on the ad group by default, or on the campaign when you set `budget_optimization` to `ad_campaign`.

## How results are counted

Every performance number Whop reports is attributed by the **Whop pixel**, not by the ad network.

`result_event` names the conversion event a campaign is judged on, and `results` is the pixel-attributed count of it. `cost_per_result` divides spend by that count. When a campaign's ad groups optimize toward different goals there's no single `result_event`, so it's `null` and `results` becomes the sum of each ad group's own attributed results. When nothing Whop-attributable is being optimized for, both are `null` rather than zero.

## Desired cost per result

A campaign's `desired_cost_per_result` is the cost per result, in USD, you want it to achieve. It's a target only: it isn't a cost cap or bid cap, and it doesn't change how the campaign's ads bid. Affiliates running the campaign see it as the cost per result to aim for. An ad group's own `desired_cost_per_result` is a different setting: that one is its cost cap or bid cap. Set the campaign's `desired_cost_per_result` to `null` to clear the target.

## Paying for ads

Spend accrues as ads deliver and is charged afterward. **Billing belongs to the account, across its campaigns.** Each collection combines eligible unpaid campaign spend into one payment using the account's configured ads payment methods. You don't configure a separate payment method or request a separate retry for each campaign. The `spend` field on a campaign is the amount charged to you, in `spend_currency`.

Collection timing depends on accrued spend and billing thresholds. Consolidation doesn't mean one payment per day. AI creative generation uses its own balance payment. Too little balance returns a `402` carrying a `deposit_url`.

### When a payment fails

An account payment failure blocks delivery across its billable campaigns. Each affected campaign reports `delivery_status: "payment_failed"`, while its configured `status` stays `active` or `paused`. Read `delivery_status` to determine whether a campaign can deliver, and `issues` for the failure reason.

The account can't activate campaigns while it has unresolved payment failures. You can still pause a campaign to keep it off after payment recovery. Whop also schedules automatic retries. You can request a retry after fixing the payment method.

### Retry a payment

1. In your account's Ads dashboard, select **Update payment method** in the failed-payment banner, or open Ads settings and select **Billing**. Update the account's card or fund the balance it uses.
2. Select **Retry payment** once for the account. API integrations should call [Retry Failed Ads Payments](https://docs.whop.com/api-reference/accounts/retry-failed-ads-payments.md) with the account's `biz_` ID and `ad_campaign:update` permission.
3. Wait for the background attempt. A `202` response with `queued: true` confirms the job was accepted, **not that the payment succeeded**. Read the account's [ad campaigns](https://docs.whop.com/api-reference/ad-campaigns/list-ad-campaigns.md) for the outcome.
4. After successful settlement, the payment block clears. Delivery can show `processing` while it's recalculated and synchronized. Campaigns configured as active can resume if their other delivery requirements are met. Campaigns configured as paused stay paused.

```bash theme={null}
curl -X POST "https://api.whop.com/api/v1/accounts/biz_YOUR_ACCOUNT/retry_ads_payment" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-09-22"
```

```json theme={null}
{ "account_id": "biz_YOUR_ACCOUNT", "queued": true }
```

Repeated requests while an account retry is queued or running return an error asking you to wait. If the payment fails again, delivery remains blocked: check the failure reason and fix the payment method before trying again. Don't loop over campaigns to retry each one.

Older campaigns whose configured `status` was already `payment_failed` return to `paused` after successful retry, because their previous active/paused setting wasn't retained.

## Why a campaign isn't delivering

`delivery_status` answers this directly. It's the first field to read when a campaign exists but nothing is happening. Several states can be true at once, so the value is the highest one on this ladder:

| `delivery_status` | What it means |
| - | - |
| `payment_failed` | Account billing failed. Delivery is blocked without changing the configured active/paused setting |
| `rejected` | Every ad in the campaign was rejected |
| `draft` | Not submitted for delivery |
| `paused` | You paused the campaign or every ad group in it, or it has no ads left. Deleting a campaign's last ad or ad group pauses it |
| `in_review` | An ad is still in content review and nothing is delivering yet |
| `learning` | Delivering, and still in its learning period |
| `processing` | The campaign hasn't reached the ad network, or every ad group is still processing |
| `revise_content` | The campaign is waiting to run and content review keeps every waiting ad off at least one ad network. Read each ad's `content_review` |
| `issues` | The campaign has open issues, or every ad has one. Read the `issues` array |
| `scheduled` | The campaign's start is in the future, or every ad group is waiting for its own start |
| `completed` | The campaign's end time has passed. A campaign with no end never completes |
| `active` | Delivering |

Ads and ad groups carry their own `delivery_status` with different values. A new ad passes through `in_review` before it delivers.

## Where to go next

> [card] [Create and launch an ad](https://docs.whop.com/api-reference/ads/overview.md)
>
> The full flow, from creative to live ad, in two calls.

> [card] [Install the pixel](https://docs.whop.com/developer/ads/pixel.md)
>
> Required before results can be attributed.

> [card] [Build an audience](https://docs.whop.com/api-reference/audiences/create-audience.md)
>
> Upload a customer list to target or exclude.

> [card] [Ads reference](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign)
>
> Every advertising endpoint, with a playground.


## Related topics

- [Ads](https://docs.whop.com/elements/latest/ads/overview.md)
- [Whop CLI](https://docs.whop.com/cli/overview.md)
- [Ads API Overview](https://docs.whop.com/api-reference/ads/overview.md)
