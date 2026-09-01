> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Partners

> Refer businesses onto Whop and earn on their activity across two tiers, with a referral link and earnings you can read from the API.

You earn for as long as the referral lasts, on four separate income sources. One call enrolls you and returns your link. Everything else reports what you've referred and what you've earned.

## What you can build

* **A referral program you run yourself**, sharing your own link with your own audience.
* **Reporting on what you referred**: which businesses and users are still active, and what each produces.
* **Earnings statements** itemized to the transaction and rate behind every line.
* **A public standings page** from the partner leaderboard.

## Core objects

Partner, referred business, partner earning. Each is defined in [Core Concepts](/developer/concepts).

## Enrolling

[`POST /partners`](/api-reference/beta/partners/enroll-as-a-whop-partner) enrolls the calling user and makes their partner businesses eligible for earnings. It returns `referral_link`, the URL you share to get credited, and `whop_partner_enabled_at`. Enrolling again keeps the original enrollment time rather than resetting it, which is what makes the call safe to repeat.

Requires the `partner:create` permission. Reading referrals and earnings requires `partner:basic:read`, and listing referred businesses also accepts `referral:basic:read`. See [Permissions](/developer/guides/permissions).

## Two tiers

`my_partner_tier` on each referred business tells you which level you earn at:

| `my_partner_tier` | You                                                                                      |
| ----------------- | ---------------------------------------------------------------------------------------- |
| `first`           | Referred the business owner onto Whop directly                                           |
| `second`          | Referred the partner who referred that owner, earning a smaller share on their referrals |

Second-tier earnings carry a `second_tier` flag. [`GET /partners/referred_users`](/api-reference/beta/partners/list-the-users-the-caller-referred) lists the users you brought on, each with the second-tier earnings their businesses have produced for you.

## What you earn on

`payout_percentages` on a referred business gives your rate per income source as a fraction, so `0.3` means 30%. Three of the four are a share of Whop's gross profit. Ad spend is the exception, and it's a share of the spend itself.

| `income_source`    | Your share is of                              |
| ------------------ | --------------------------------------------- |
| `sales`            | Whop's profit on the business's product sales |
| `ad_spend`         | The business's Whop Ads spend                 |
| `transfer`         | Whop's profit from platform balance transfers |
| `card_interchange` | Whop's profit from card interchange           |

[`GET /partners/businesses`](/api-reference/beta/partners/list-referred-businesses) lists everything you've referred, newest first. Each partner business carries a `coma_` ID, a `status` of `active` or `removed`, and `referral_started_at` and `referral_expires_at`.

`earnings_usd` and `volume_usd` are objects rather than single numbers. `earnings_usd` breaks into `completed`, `pending`, and `total`. `volume_usd` breaks into `attributed`, `awaiting_settlement`, and `settled`.

## Reading earnings

[`GET /partners/businesses/{id}/earnings`](/api-reference/beta/partners/list-referred-business-earnings) itemizes what Whop pays you for one business. Each earning names its `income_source`, the `transaction_amount_usd` it came from, and your `payout_percentage`.

Earnings settle before they pay. Both `commission_amount_usd` and `payout_percentage` are `null` until they do.

| `status`              | Meaning                                               |
| --------------------- | ----------------------------------------------------- |
| `awaiting_settlement` | The underlying transaction hasn't settled yet         |
| `pending`             | Settled, not yet paid out                             |
| `completed`           | Paid out                                              |
| `canceled`            | Voided before payout, with `cancelation_reason`       |
| `reversed`            | Clawed back after the fact, with `cancelation_reason` |

Where the data exists, `financial_activity` breaks out the income and cost lines behind the commission. It's `null` for earnings that settled before Whop recorded that detail.

## The leaderboard

[`GET /partners/leaderboard`](/api-reference/beta/partners/retrieve-the-leaderboard) ranks referrers by partner earnings. The `period` parameter takes `day`, `month`, `year`, `last_30_days`, or `all_time`, and defaults to `all_time`.

Authentication is optional. Anonymous callers get the rankings in `leaders`, and authenticated callers also get their own standing in `me`, which is `null` when you have no referral earnings yet.

## Where to go next

<Columns cols={2}>
  <Card title="Enroll as a partner" icon="handshake" href="/api-reference/beta/partners/enroll-as-a-whop-partner">
    One call to join and get your referral link.
  </Card>

  <Card title="List your referrals" icon="list" href="/api-reference/beta/partners/list-referred-businesses">
    Every business you brought on, with earnings.
  </Card>

  <Card title="Affiliates" icon="link" href="/developer/guides/affiliates">
    Referral tracking inside your own products.
  </Card>

  <Card title="Partners reference" icon="code" href="/api-reference/beta/partners/partner">
    Every partner endpoint, with a playground.
  </Card>
</Columns>
