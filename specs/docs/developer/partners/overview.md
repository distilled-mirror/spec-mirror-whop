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

# Whop Partners API

> Refer businesses onto Whop and earn on their activity, with a referral link and earnings you can read from the API.

You earn for as long as the referral lasts, on four separate income sources. One call enrolls you and returns your link. Everything else reports what you've referred and what you've earned.

## What you can build

* **A referral program you run yourself**, sharing your own link with your own audience.
* **Reporting on what you referred**: which businesses and users are still active, and what each produces.
* **Earnings statements** itemized to the transaction and rate behind every line.
* **A public standings page** from the partner leaderboard.

## Core objects

Partner, referred business, partner earning. Each is defined in [Core Concepts](https://docs.whop.com/developer/concepts.md).

## Enrolling

[`POST /partners`](https://docs.whop.com/api-reference/partners/enroll-as-a-whop-partner) enrolls the calling user and makes their partner businesses eligible for earnings. It returns `referral_link`, the URL you share to get credited, and `whop_partner_enabled_at`. Enrolling again keeps the original enrollment time rather than resetting it, which is what makes the call safe to repeat.

Requires the `partner:create` permission. Reading referrals and earnings requires `partner:basic:read`, and listing referred businesses also accepts `referral:basic:read`. See [Permissions](https://docs.whop.com/developer/guides/permissions.md).

## What you earn on

`payout_percentages` on a referred business gives your rate per income source as a fraction, so `0.3` means 30%. Three of the four are a share of Whop's gross profit. Ad spend is the exception, and it's a share of the spend itself.

| `income_source` | Your share is of |
| - | - |
| `sales` | Whop's profit on the business's product sales |
| `ad_spend` | The business's Whop Ads spend |
| `transfer` | Whop's profit from platform balance transfers |
| `card_interchange` | Whop's profit from card interchange |

On a business closed for a partner business, that rate belongs to the team. [`GET /partners/businesses/:id`](https://docs.whop.com/api-reference/partners/retrieve-a-referred-business) also returns `my_payout_percentages`, your own cut of each rate. It's the whole rate when you referred the business yourself. On a partner business's deal it's your team share of the rate. A manager earns their share minus the share of the person below them, and a deal participant earns their cut. It's `null` when you earn nothing on the business.

[`GET /partners/businesses`](https://docs.whop.com/api-reference/partners/list-referred-businesses) lists everything you've referred, newest first. Each partner business carries a `coma_` ID, a `status` of `active` or `removed`, and `referral_started_at` and `referral_expires_at`.

`earnings_usd` and `volume_usd` are objects rather than single numbers. `earnings_usd` breaks into `completed`, `pending`, and `total`. `volume_usd` breaks into `attributed`, `awaiting_settlement`, and `settled`.

## Reading earnings

[`GET /partners/businesses/{id}/earnings`](https://docs.whop.com/api-reference/partners/list-referred-business-earnings) itemizes what Whop pays you for one business. Each earning names its `income_source`, the `transaction_amount_usd` it came from, and your `payout_percentage`.

Earnings settle before they pay. Both `commission_amount_usd` and `payout_percentage` are `null` until they do.

| `status` | Meaning |
| - | - |
| `awaiting_settlement` | The underlying transaction hasn't settled yet |
| `pending` | Settled, not yet paid out |
| `completed` | Paid out |
| `canceled` | Voided before payout, with `cancelation_reason` |
| `reversed` | Clawed back after the fact, with `cancelation_reason` |

Where the data exists, `financial_activity` breaks out the income and cost lines behind the commission. It's `null` for earnings that settled before Whop recorded that detail.

## The leaderboard

[`GET /partners/leaderboard`](https://docs.whop.com/api-reference/partners/retrieve-the-leaderboard) ranks referrers by partner earnings. The `period` parameter takes `day`, `month`, `year`, `last_30_days`, or `all_time`, and defaults to `all_time`.

Authentication is optional. Anonymous callers get the rankings in `leaders`, and authenticated callers also get their own standing in `me`, which is `null` when you have no referral earnings yet.

## Where to go next

> [card] [Enroll as a partner](https://docs.whop.com/api-reference/partners/enroll-as-a-whop-partner.md)
>
> One call to join and get your referral link.

> [card] [List your referrals](https://docs.whop.com/api-reference/partners/list-referred-businesses.md)
>
> Every business you brought on, with earnings.

> [card] [Affiliates](https://docs.whop.com/developer/guides/affiliates.md)
>
> Referral tracking inside your own products.

> [card] [Partners reference](https://docs.whop.com/api-reference/partners/partner)
>
> Every partner endpoint, with a playground.


## Related topics

- [Partners API](https://docs.whop.com/api-reference/partners/partner.md)
- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [Enroll as a Whop Partner](https://docs.whop.com/api-reference/partners/enroll-as-a-whop-partner.md)
