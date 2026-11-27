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

# Stats API Reference

> Stats represent aggregated activity for an account over time.

They help you understand revenue, transactions, disputes, members, referrals, and advertising performance across reporting periods like days, weeks, or months.

Use the Stats API to list the metrics you can chart and retrieve one as a time series over a date range, or list reports and retrieve aggregates that are not bucketed over time, such as platform-wide payment trends.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Reports](https://docs.whop.com/api-reference/stats/list-reports.md) | GET `/stats/reports` |
| [Retrieve Partner Revenue Lines](https://docs.whop.com/api-reference/stats/retrieve-partner-revenue-lines.md) | GET `/stats/reports/partner_revenue_lines` |
| [Retrieve Platform Trends](https://docs.whop.com/api-reference/stats/retrieve-platform-trends.md) | GET `/stats/reports/platform_trends` |
| [List Time Series Metrics](https://docs.whop.com/api-reference/stats/list-time-series-metrics.md) | GET `/stats/time_series` |
| [Retrieve Metric Time Series](https://docs.whop.com/api-reference/stats/retrieve-metric-time-series.md) | GET `/stats/time_series/{metric}` |

## Metrics glossary

Every metric has a `key` that you pass as the `{metric}` path segment to [Retrieve Metric Time Series](https://docs.whop.com/api-reference/stats/retrieve-metric-time-series.md). [List Time Series Metrics](https://docs.whop.com/api-reference/stats/list-time-series-metrics.md) returns the same catalog at runtime, with each metric's `unit` and the `properties` you can filter or break down by. An unknown key returns a `404`.

### Units

| Unit | How to read the value |
| - | - |
| `count` | An integer count. |
| `currency` | A decimal amount in the display currency (see [`convert_to`](#query-parameters)), for example `4899.50`. |
| `percent` | A number where `1.6` means 1.6%, already scaled to percentage points rather than a `0`–`1` fraction. |

### Filtering and breaking down

Each property works two ways unless noted otherwise:

* **Filter** — pass `property=value` to narrow the series to a single value, for example `?payment_method=card`.
* **Break down** — pass `breakdown_by=property` to split each point into one entry per value. For example, `?breakdown_by=currency` returns an entry for `usd`, an entry for `eur`, and so on. One `breakdown_by` at a time.

Passing a property a metric doesn't list returns a `400`.

| Property | Values |
| - | - |
| `currency` | A currency code from the International Organization for Standardization (ISO) 4217, such as `usd` or `eur`. For `market_prices`, it can also be an asset symbol such as `btc` or `xaut`. Transaction metrics report the original currency without conversion. |
| `hostname` | The website hostname associated with the traffic, for example `shop.example.com` (traffic metrics only). |
| `page` | A hostname plus normalized path, for example `shop.example.com/pricing` (traffic metrics only). |
| `source` | Where a visit came from: `whop_ads` for traffic from Whop ads, `direct` for unattributed visits, or the visit's `utm_source` value (traffic metrics only). |
| `device_type` | `desktop`, `mobile`, `tablet`, or `unknown` (traffic metrics only). |
| `country_code` | An uppercase ISO 3166-1 alpha-2 code, for example `US`. Empty when unknown (traffic metrics only). |
| `payment_method` | `card` or `crypto`. |
| `issuer` | The bank that issued the card, exactly as the processor reports it, for example `JPMORGAN CHASE BANK N A`, or `unknown` for payments with no reported bank. Filter only. `auth_rate` only. |
| `card_network` | A card brand, for example `visa` or `mastercard`. A refinement of `payment_method=card` — only valid alongside it. |
| `dispute_reason` | A provider-normalized dispute reason such as `product_not_received`, `product_unacceptable`, `fraudulent`, or `credit_not_processed`. Use it on `snapshot_disputes`. Clients can group remaining reason slugs into “Other” for display. |
| `product` | A product ID to scope the metric to a single product. |
| `fee_type` | A fee line type, for example `payment_processing_percentage_fee` (see the [Ledger Activity fees glossary](https://docs.whop.com/api-reference/ledgers/ledger-activity#fees)). |
| `status` | A membership status for New users, or an affiliate payment status for Partner metrics. |
| `access_level` | A member's access level, for example `customer` (New users only). |
| `most_recent_action` | A member's most recent lifecycle action (New users only). |
| `account_id` | A referred business ID (Partner metrics only). |
| `risk` | `canceling` for a membership set to cancel at the end of its period, or `past_due` (at-risk renewal metrics only). |
| `cancel_option` | The reason category the member picked when canceling, for example `too_expensive`, or `none` when they picked no reason (cancellation metrics only). |
| `tier` | `first` or `second` (Partner metrics only). |
| `referred_user_id` | A referred user ID. Filter-only. You can't use it with `breakdown_by` (Partner metrics only). |
| `merchant` | A slugified merchant name, for example `starbucks`, or `other` when unknown (card metrics only). On `card_spend`, it names the transaction's merchant. On cashback metrics, it names the cashback offer's merchant bucket. |
| `event_name` | A tracked event name, for example `pixel.page` or `pixel.custom` (`events` only). |
| `event_type` | A canonical group of events: `page_view` (pixel page views plus whop.com store views), `checkout_start` (hosted and embedded checkout views), or `other` (`events` only). |
| `custom_name` | The merchant-defined name of a custom event (`events` only). A refinement of `event_name=pixel.custom` — only valid alongside it, since no other event carries a custom name. |

### Query parameters

These apply to every metric on [Retrieve Metric Time Series](https://docs.whop.com/api-reference/stats/retrieve-metric-time-series.md), independent of its properties:

| Parameter | Description |
| - | - |
| `from`, `to` | Required. The date range (`YYYY-MM-DD`) to plot. |
| `interval` | Point width: `hour`, `day`, `week`, or `month`. Defaults to `day`. Traffic metrics also accept `minute` for ranges up to one hour. `market_prices` accepts `five_minutes`, `thirty_minutes`, `hour`, or `day`. |
| `convert_to` | Display currency for money metrics. The API converts every amount into this ISO currency at each period's rate. Defaults to `usd`. Filtering or breaking down by `currency` reports the original currency and ignores this parameter. `market_prices` currently supports only `usd`. |
| `time_zone` | An Internet Assigned Numbers Authority (IANA) time zone to group the series, for example `America/New_York`. Defaults to Coordinated Universal Time (`UTC`). |
| `breakdown_by` | Split each point out by one of the metric's properties. |
| `snapshot_window` | Snapshot metrics only. Ordinary snapshots accept `30d` as their trailing activity window. Cohorted dispute metrics accept `7d` or `28d` as the sales-transaction pooling window. The metric name fixes the attribution window. Card network program metrics accept `7d`, `14d`, `28d`, or `56d` and default to `28d`. Check the metric catalog's `windows` array for accepted values. |

### Revenue

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `net_revenue` | `currency` | — | Earnings kept after refunds, disputes, and all fees. |
| `gross_revenue` | `currency` | `payment_method`, `product`, `currency` | Gross earnings from sales and platform income before refunds or fees. |
| `net_volume` | `currency` | — | Gross earnings minus Whop fees before refunds and other deductions. |
| `gross_transaction_value` | `currency` | `currency` | Gross Merchandise Value (GMV): total value of completed sales before refunds or fees. |
| `marketplace_revenue` | `currency` | `payment_method`, `product`, `currency` | Revenue from marketplace sales, excluding direct-to-consumer. |
| `churned_revenue` | `currency` | — | Recurring revenue lost from memberships whose monthly recurring revenue decreased over the period. |
| `total_refunded` | `currency` | `payment_method`, `product`, `currency` | Total amount refunded to buyers. |
| `monthly_recurring_revenue` | `currency` | `currency` | Monthly recurring revenue from active subscriptions. |
| `annual_recurring_revenue` | `currency` | `currency` | Annualized recurring revenue: monthly recurring revenue times twelve. |
| `average_revenue_per_user` | `currency` | `payment_method`, `product`, `currency` | Average revenue earned per paying user. |
| `average_revenue_per_subscription` | `currency` | `payment_method`, `product`, `currency` | Average revenue earned per active subscription. |

### Payments

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `successful_payments` | `count` | `payment_method`, `product`, `currency` | Successful payments received. |

### Fees

Every fee metric accepts a `currency` filter.

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `fees` | `currency` | `currency`, `fee_type` | All fees charged. Break down by `fee_type` to itemize them. |
| `payment_processing_fees` | `currency` | `currency` | Card, network, currency-conversion, billing, and payout fees. |
| `whop_processing_fees` | `currency` | `currency` | Whop's processing fee on transactions. |
| `affiliate_fees` | `currency` | `currency` | Fees paid to affiliates on referred sales. |
| `marketplace_fees` | `currency` | `currency` | Affiliate fees on marketplace sales. |
| `dispute_fees` | `currency` | `currency` | Fees charged for disputes and dispute alerts. |
| `sales_tax_withheld` | `currency` | `currency` | Sales tax withheld and remitted. |

### Disputes & refunds

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `disputes` | `count` | — | Disputes opened for the account. |
| `dispute_alerts` | `count` | — | Early warning dispute alerts received on the account's payments. |
| `dispute_rate` | `percent` | `payment_method` | Share of paid receipts with a dispute. |
| `refund_rate` | `percent` | `payment_method` | Share of paid receipts refunded. |

### Authorizations

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `auth_rate` | `percent` | `issuer` (filter only), `product` | Share of decided card authorizations the processor approved. |

### Members & users

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `new_users` | `count` | `status`, `access_level`, `most_recent_action`, `product` | Users who joined. |
| `users_growth` | `count` | — | Active members over time. |
| `paid_active_members` | `count` | — | Active paying members. |
| `churn_rate` | `percent` | — | Share of paying members who canceled. |
| `trial_conversion_rate` | `percent` | — | Share of trials that converted to paid. |

### Renewals & cancellations

Money in these metrics is the plan's renewal price, before discounts. A yearly plan counts at its full price on the day it renews. `from` and `to` can be in the future.

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `upcoming_renewal_revenue` | `currency` | `currency`, `product` | Money due to renew from active memberships that are not set to cancel and not paused, by the day they renew. |
| `upcoming_renewals` | `count` | `currency`, `product` | Active memberships due to renew that are not set to cancel and not paused, by the day they renew. |
| `at_risk_renewal_revenue` | `currency` | `currency`, `product`, `risk` | Renewal money that may not arrive: memberships set to cancel at the end of their period, and memberships that are past due. |
| `at_risk_renewals` | `count` | `currency`, `product`, `risk` | Memberships set to cancel at the end of their period, and memberships that are past due, by the day their period ends. |
| `canceled_renewal_revenue` | `currency` | `currency`, `product`, `cancel_option` | Renewal money lost to cancellations: the price of the next renewal of each paid membership canceled in the period. |
| `cancellations` | `count` | `currency`, `product`, `cancel_option` | Paid memberships canceled in the period. |
| `cancellations_with_written_reason` | `count` | `currency`, `product`, `cancel_option` | Paid memberships canceled in the period whose member also wrote a reason in their own words. The text itself is not returned. |

### Partners

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `partner_earnings` | `currency` | `status`, `account_id`, `tier`. `referred_user_id` is filter-only | Commissions earned from referred businesses across both partner tiers. Whop Sales commission counts only the share that reaches its people. In other organizations, reads scoped to one person count that person's team share. That covers a partner's own read, one rep, one lead's line and a team member's own line. Advances are left out unless filtered by `income_source`. |
| `partner_projected_earnings` | `currency` | `status`, `account_id`, `tier`. `referred_user_id` is filter-only | Earned commissions plus estimates for sales still settling, counted at the same shares as `partner_earnings`. Advances are left out unless filtered by `income_source`. |
| `partner_volume` | `currency` | `status`, `account_id`, `tier`. `referred_user_id` is filter-only | Sales volume generated by referred businesses across both partner tiers. |
| `partner_payouts` | `currency` | `status`, `account_id`, `tier`. `referred_user_id` is filter-only | Completed commissions from referred businesses, bucketed by payout date. |

### Cards

Card metrics report spend and cashback on the account's issued Whop cards. They use only United States dollars and Coordinated Universal Time (UTC). They don't accept `convert_to` or `time_zone`. Accounts without issued cards return an empty series.

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `card_spend` | `currency` | `merchant` | Total spend on issued cards each period. Break down by `merchant` to split by merchant. |
| `cashback` | `currency` | `merchant` | Cashback earned on eligible card spend. Break down by `merchant` to split by cashback offer. |
| `cashback_qualified_spend` | `currency` | `merchant` | Card spend that earned cashback. Break down by `merchant` to split by cashback offer. |

### Market data

`market_prices` is publicly available without authentication, `account_id`, or `user_id`. It returns historical prices for Bitcoin (`btc`) and Gold (`xaut`) in United States dollars. It supports ranges up to one year and returns at most 2,000 points.

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `market_prices` | `currency` | `currency` | Market price of Bitcoin or Gold in United States dollars over time. |

### Advertising & traffic

Traffic metrics cover pixel events on the merchant's own websites plus whop.com store views, support `interval=minute` for ranges up to one hour, and cap their range at 30 days. High-cardinality breakdowns (such as `page`) return at most the top 500 entries per point.

| Metric | Unit | Filters & breakdowns | Description |
| - | - | - | - |
| `ad_spend` | `currency` | — | Total advertising spend across the account's ad campaigns. |
| `page_visits` | `count` | — | Store page views for the account. |
| `events` | `count` | `event_name`, `custom_name`, `event_type`, `hostname`, `page`, `source`, `device_type`, `country_code` | Tracked events across the account's traffic, excluding crawlers. Filter by `event_type=page_view` or `event_type=checkout_start` for canonical counts, `event_name` for one raw event. |
| `people` | `count` | `hostname`, `page`, `source`, `device_type`, `country_code` | Unique people across the account's traffic, deduplicated by resolved identity. |

### Snapshot metrics

> [!NOTE]
>
> Snapshot metrics are captured once per day and preserve what was known at that
> time. They are day-only and UTC-only: `interval` must be `day` and `time_zone`
> is not accepted. You still pass `from` and `to` to choose the range of daily
> points. Use each metric's `windows` catalog field to determine which
> `snapshot_window` values it accepts.

| Metric | Unit | `snapshot_window` | Filters & breakdowns | Description |
| - | - | - | - | - |
| `snapshot_receipts` | `count` | `30d` | `card_network`, `payment_method` | Paid receipts in the trailing 30 days. |
| `snapshot_refunds` | `count` | `30d` | — | Refunds issued in the trailing 30 days. |
| `snapshot_refund_rate` | `percent` | `30d` | — | Share of trailing-30-day paid receipts refunded. |
| `snapshot_disputes` | `count` | `30d` | `card_network`, `payment_method`, `dispute_reason` | Disputes outside the Rapid Dispute Resolution (RDR) program filed in the trailing 30 days. Break down by normalized reason to build a dispute-composition chart. |
| `snapshot_dispute_rate` | `percent` | `30d` | `card_network`, `payment_method` | Disputes filed in the trailing 30 days divided by paid receipts in that window. |
| `visa_dispute_fraud_report_rate` | `percent` | `30d` | — | Visa disputes plus Visa fraud reports in the trailing 30 days divided by Visa paid receipts in that window. A payment with both counts twice. Not reported on a day without Visa payments, and not capped at 100. |
| `projected_dispute_rate` | `percent` | `28d` | `card_network`, `payment_method` | Where the dispute rate of card payments 8 to 35 days old is heading, projected from the disputes and Visa fraud reports of each payment's first 7 days. Reported for Visa and Mastercard. Name a card network as a filter or breakdown within `payment_method=card`. Not reported for a network with fewer than 10 early events. |
| `bank_return_rate` | `percent` | `30d` | `payment_method` | ACH or SEPA debits the bank took back after they were paid in the trailing 30 days, divided by the debits that became paid in that window. Name a bank method (`us_bank_account`, `sepa_debit`) as a filter or breakdown. Not reported without a paid debit in the window, and not capped at 100. |
| `snapshot_cohorted_dispute_rate_7d_attr` | `percent` | `7d`, `28d` | `card_network`, `payment_method` | Share of the latest mature sales cohort disputed within 7 days of purchase. |
| `snapshot_cohorted_dispute_rate_14d_attr` | `percent` | `7d`, `28d` | `card_network`, `payment_method` | Share of the latest mature sales cohort disputed within 14 days. |
| `snapshot_cohorted_dispute_rate_28d_attr` | `percent` | `7d`, `28d` | `card_network`, `payment_method` | Share of the latest mature sales cohort disputed within 28 days. |
| `snapshot_vamp_rate` | `percent` | `7d`, `14d`, `28d`, `56d` | — | Visa disputes plus issuer TC40 fraud reports, counted once per payment, divided by Visa sales in the window. |
| `snapshot_mastercard_ecm_rate` | `percent` | `7d`, `14d`, `28d`, `56d` | — | Mastercard chargebacks in the window divided by Mastercard sales in the window before it. |
| `snapshot_mastercard_efm_rate` | `percent` | `7d`, `14d`, `28d`, `56d` | — | Mastercard fraud chargebacks in the window divided by Mastercard sales in the window before it. |
| `snapshot_amex_fraud_rate` | `percent` | `7d`, `14d`, `28d`, `56d` | — | Amex fraud chargeback dollars in the window divided by Amex sales dollars in the window. |
| `snapshot_vamp_disputes` | `count` | `7d`, `14d`, `28d`, `56d` | — | Visa disputes outside RDR whose first dispute is not fraud, counted once per payment in the window, part of the VAMP numerator. |
| `snapshot_vamp_fraud_reports` | `count` | `7d`, `14d`, `28d`, `56d` | — | Visa payments with a TC40 fraud report or a fraud chargeback, counted once per payment in the window, part of the VAMP numerator. |
| `snapshot_vamp_payments` | `count` | `7d`, `14d`, `28d`, `56d` | — | Visa sales in the window, the denominator of the VAMP ratio. |
| `snapshot_mastercard_efm_fraud_disputes` | `count` | `7d`, `14d`, `28d`, `56d` | — | Mastercard fraud chargebacks in the window, the numerator of the Mastercard EFM ratio. |
| `snapshot_mastercard_efm_prior_sales` | `count` | `7d`, `14d`, `28d`, `56d` | — | Mastercard sales in the window before it, the denominator of the Mastercard EFM ratio. |
| `snapshot_amex_fraud_dispute_volume` | `currency` | `7d`, `14d`, `28d`, `56d` | — | USD value of Amex fraud chargebacks in the window, the numerator of the Amex fraud full recourse ratio. |
| `snapshot_amex_sales_volume` | `currency` | `7d`, `14d`, `28d`, `56d` | — | USD value of Amex sales in the window, the denominator of the Amex fraud full recourse ratio. |
| `snapshot_resolution_center_cases` | `count` | `30d` | — | Resolution Center cases opened in the trailing 30 days. |
| `snapshot_resolution_center_case_rate` | `percent` | `30d` | — | Trailing-30-day Resolution Center cases divided by paid receipts. |
| `snapshot_approved_authorizations` | `count` | `30d` | — | Card authorizations approved in the trailing 30 days. |
| `snapshot_decided_authorizations` | `count` | `30d` | — | Card authorizations approved or declined in the trailing 30 days. |
| `snapshot_auth_rate` | `percent` | `30d` | — | Trailing-30-day approved authorizations divided by decided authorizations. |

### Card network program snapshots

These metrics track a business against the card networks' monitoring programs: Visa's Acquirer Monitoring Program (VAMP), Mastercard's Excessive Chargeback (ECM) and Excessive Fraud (EFM) programs, and American Express Fraud Full Recourse. Each point covers the trailing `snapshot_window` ending at the capture time. Disputes exclude the Rapid Dispute Resolution (RDR) program, and fraud chargebacks are disputes with the normalized reason `fraudulent`. Mastercard metrics divide by sales in the preceding window of the same length, as Mastercard divides by the prior month's transactions. A day with no sales in the denominator window has no point.

### Cohorted dispute snapshots

Cohorted metrics attribute each dispute to the date of its original paid receipt. The attribution window in the metric name controls how long after purchase a dispute can count. The `snapshot_window` controls how many consecutive sales days contribute to a point. Use `7d` for a more reactive series or `28d` for a smoother denominator.

Each captured point uses the latest sales cohort whose complete attribution window has elapsed. For a snapshot captured on date `D`, its cohort ends on `D - attribution days - 1 day`. The extra day ensures the final attribution day is complete. To reproduce a cohort-end-date chart, plot each point on that inferred cohort end date. Comparing 7d, 14d, and 28d attribution values at the same raw API timestamp compares different mature cohorts. Align them by cohort end date before expecting `7d ≤ 14d ≤ 28d`.


## Related topics

- [Stats API](https://docs.whop.com/developer/guides/stats.md)
- [API Logs Reference](https://docs.whop.com/api-reference/api-logs/api-log.md)
- [API Keys Reference](https://docs.whop.com/api-reference/api-keys/api-key.md)
