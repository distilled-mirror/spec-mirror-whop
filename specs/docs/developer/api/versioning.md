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

# API Versions

> Pin your requests to a dated API version so changes to the API never break your integration.

The Current API is versioned with dates. Breaking changes ship only inside a new dated version that you opt into. Pin a version and the request and response shapes you built against never change. Official SDKs automatically send the version used to generate them.

```bash theme={null}
curl https://api.whop.com/api/v1/variants \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Api-Version-Date: 2026-07-23"
```

| You send | You get |
| - | - |
| `Api-Version-Date: 2026-07-23` | The API as it worked on that date |
| No header, API key has a pin | The API version stored on the API key |
| No header or stored pin | The original `2025-01-01` behavior |
| An unknown date | A `400` error with the supported versions |

> [!NOTE]
>
> **If you don't pass an `Api-Version-Date` or have a stored API-key pin, the stable API model is used.** Requests with neither are served by the pre-versioning behavior, so existing integrations keep working unchanged. Pin a dated version to opt into the latest API.

## API key version pins

API keys carry their own API version pin. Requests authenticated with an API key use that pin when they omit `Api-Version-Date`. Existing keys without a stored pin use `2025-01-01`. Newly created keys use the latest released version.

An explicit `Api-Version-Date` header always takes precedence over the API key's pin, so you can test an upgrade before changing the saved version.

Every version automatically gets new endpoints and optional fields. Breaking changes create a new dated version, which the changelog below lists.

When upgrading, review every entry after your current pin through your target version, not just entries from the target month. In particular, upgrading from `2026-07-20` to September crosses the `2026-07-23` members and memberships migration.

## Changelog

The TypeScript SDK releases listed below use the API version in that entry by default. Overriding `apiVersionDate` changes the response version, but not the SDK types.

**2026-11-25**

Account reserves list what a platform holds for its subaccounts

On [List Account Reserves](https://docs.whop.com/api-reference/accounts/list-account-reserves.md), `by_type` can include the type `subaccount_negative_balances`. It's the part of a platform's balance that's held for the negative balances of its subaccounts. It has no release date, so it doesn't appear in `unlocks_by_date`. Earlier versions leave this entry out of `by_type`, and `amount` still counts it.

**2026-11-23-3**

Dispute alerts rename `category` to `reason`

On [dispute alerts](https://docs.whop.com/api-reference/dispute-alerts/dispute-alert), the `category` field is now `reason`. Its values don't change: `fraud`, `customer_dispute`, or `null`. The `dispute_alert.created` webhook payload uses the new name too.

**2026-11-23-2**

Payment method domains stop responding after December 23

The payment method domains endpoints stop responding after December 23, 2026. From December 24, every version returns a `410`. Until then each response carries `Deprecation` and `Sunset` headers. The [migration guide](https://docs.whop.com/developer/api/migrate-from-previous-versions.md#payment-method-domains) maps each request to the [domains](https://docs.whop.com/api-reference/domains/domain) API.

**2026-11-23-1**

Audiences drop ad network fields

Audiences no longer return `platform_audience_ids` or `match_rates`. [Create Audience](https://docs.whop.com/api-reference/audiences/create-audience.md) returns a `400` when `column_mapping` includes `ltv`.

**2026-11-23**

Lifetime budgets have ended

New ad campaigns and ad groups can't use a lifetime budget. This applies to every API version, not only this one.

[Create an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/create-an-ad-campaign.md), [Create an Ad Group](https://docs.whop.com/api-reference/ad-groups/create-an-ad-group.md), and [Create an Ad](https://docs.whop.com/api-reference/ads/create-an-ad.md) reject `budget_type: lifetime` with a `400` whose `code` is `lifetime_budgets_retired`. An update can't switch a daily budget to lifetime either. Leave out `budget_type`, or send `daily`.

[Duplicate an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/duplicate-an-ad-campaign.md) refuses a campaign with a lifetime budget, or with an ad group on one. [Duplicate an Ad Group](https://docs.whop.com/api-reference/ad-groups/duplicate-an-ad-group.md) refuses an ad group with a lifetime budget. Both return a `400` whose `code` is `lifetime_budget_duplicate_unavailable`.

Campaigns and ad groups that already have a lifetime budget keep it, still return `budget_type: lifetime`, and can still change their `budget_amount`.

**2026-11-22-1**

Email notification preferences for team topics

* Notification preferences accept the `email` channel for a team topic. Pass `team_account_id` and `topic_id` with it. The topic's `email_delivery` must be `optional`.
* Notification topics return `email_delivery`, `push_delivery` and `every_channel_covers_email`.
* A `mobile` or `in_app` preference on a topic whose `push_delivery` is `unavailable` returns a 400.

**2026-11-22**

Ad campaigns retire engagement and bring back awareness

New campaigns can't use the `engagement` objective. Sending it to [Create an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/create-an-ad-campaign.md) or [Create ad](https://docs.whop.com/api-reference/ads/create-an-ad.md) returns a `400`. Existing engagement campaigns keep it.

On every version, the `awareness` objective is available again. Its results are impressions, and its cost per result is per 1,000 impressions. An ad group's `desired_cost_per_result` now works with any `bid_type`, and it overrides the campaign's.

**2026-11-20**

Standardize API union discriminators

* Notification preference responses use `type` to identify each preference shape.
* Automation actor references use `resource_type` to identify the referenced resource.
* Automation step senders use `type` to distinguish an email address from a user.
* A member’s `last_activity.type` identifies the activity.
* Search results use `record.type` to name the record variant.

**2026-11-19-2**

Ad group city keys

A city in an ad group's `regions` is keyed by its state or country and name, such as `US-TX:Austin`. [Create ad group](https://docs.whop.com/api-reference/ad-groups/create-an-ad-group.md) and [Update ad group](https://docs.whop.com/api-reference/ad-groups/update-an-ad-group.md) take this key. A numeric city key that an ad group already has stays valid, and the ad group returns it unchanged.

Searching targeting options returns cities and zips with these keys. It no longer returns `neighborhood`, `subcity`, `medium_geo_area`, `district` or `dma` locations, and `location_types` takes only `country`, `region`, `city` and `zip`. Both changes apply on every version.

**2026-11-19-1**

Payment quotes refuse a wrong ZIP code or tax ID

[Create payment quote](https://docs.whop.com/api-reference/payment-quotes/create-payment-quote.md) returns a `400` error for a ZIP code outside the buyer's state. It does the same for a tax ID that isn't valid. Before, it returned a quote with `tax_status: unavailable`, which a payment refuses.

The error `code` is `zip_code_outside_state`, `zip_code_state_mismatch` or `customer_tax_id_invalid`. Its `message` tells the buyer what to fix.

**2026-11-19**

User preferences drop `show_earnings`

`GET /users/me/preferences` no longer returns `show_earnings`, and `PATCH /users/me/preferences` no longer accepts it.

**2026-11-17-3**

External accounts drop `error`, and refreshing one is deprecated

[List External Accounts](https://docs.whop.com/api-reference/external-accounts/list-external-accounts.md) and the other external account endpoints no longer return `error`.

[Refresh](https://docs.whop.com/api-reference/external-accounts/refresh.md) is deprecated. It returns the external account unchanged, and its responses carry a `Deprecation` header.

**2026-11-17-2**

A payment's `paid_at` is null unless it's paid

`paid_at` on a payment is now null unless its `status` is `paid`. This covers every payments endpoint, payment webhooks, and payment results from search. Refunded payments keep `status: paid`, so they still return `paid_at`.

**2026-11-17-1**

Choose which channel members to include

[List channels](https://docs.whop.com/api-reference/channels/list-channels.md) now accepts `current`, `current_and_deleted` or `null` for `include_members`, instead of a boolean.

Replace `true` with `current` to include current members. Use `current_and_deleted` to also include former members, marked with `deleted_at`. Replace `false` with `null`, or omit the parameter, to leave out members.

**2026-11-17**

A level keeps only its level and points, and Economic Intelligence is deprecated

`level` on [Retrieve user](https://docs.whop.com/api-reference/users/retrieve-user.md) and [Retrieve account](https://docs.whop.com/api-reference/accounts/retrieve-account.md) now has `current`, `points`, `points_by_source` and `resets_at`, plus `account_level` on an account.

These fields are removed from `level`: `levels`, `points_to_next`, `monthly_qualifying_spend`, `monthly_card_spend`, `monthly_ads_spend` and `monthly_trading_volume`. `boosted` is removed from an account's `level`. An account is boosted when its `account_level` is below `current`.

`GET /economic_intelligence` and `PATCH /economic_intelligence/{id}` are deprecated. Use `GET /recommended_actions` and `POST /recommended_actions/{id}/dismiss` instead. Their responses carry `Deprecation` and successor `Link` headers.

On every version, `PATCH /economic_intelligence/{id}` accepts only `status: superseded`. Both endpoints return a `400` for `input`, `sentiment`, `user_feedback`, `result_id`, `result_page` or `result_url`. A recommendation returns only `id`, `account_id`, `action_type`, `title`, `reasoning`, `status`, `result_url`, `run_by_user_id` and its timestamps. `POST /economic_intelligence` and `POST /economic_intelligence/{id}` return a `404`.

**2026-11-16-2**

A level's `applied_level` is now `current`

`level.applied_level` is renamed to `level.current` on [Retrieve user](https://docs.whop.com/api-reference/users/retrieve-user.md) and [Retrieve account](https://docs.whop.com/api-reference/accounts/retrieve-account.md). The value is unchanged: the level that applies, from `0` up to `10`.

**2026-11-16-1**

The referred users list stops responding after November 17

`GET /partners/referred_users` stops responding after November 17, 2026. From November 18, every version returns a `410`. Until then each response carries `Deprecation` and `Sunset` headers. The list has no replacement.

**2026-11-16**

Attribution requests to a user have ended

Partners can no longer send an attribution request to a user. This applies to every API version, not only this one.

[Create a partner referral request](https://docs.whop.com/api-reference/partner-referral-requests/create-a-partner-referral-request.md) rejects `target_user_id`, `target_username` and `target_email` with a `400` whose `code` is `user_referral_requests_retired`. Send `account_id` or `account_url` to request attribution for a business. Requests that were sent to a user are no longer listed or retrieved, the `target_user_id` filter on [list partner referral requests](https://docs.whop.com/api-reference/partner-referral-requests/list-partner-referral-requests.md) returns no requests, and `user` on a request is always `null`.

**2026-11-14-1**

A user's level is public

[Retrieve user](https://docs.whop.com/api-reference/users/retrieve-user.md) returns `level` for any user whose accounts are in Whop Levels, not only for yourself. `level.applied_level` is the user's level, from `0` up to `10`.

The other fields in `level`, such as `points`, `points_by_source`, the `monthly_*` amounts and `levels`, are `null` unless you retrieve yourself with the reserved ID `me` and have balance-read scope. Check them for `null` before reading them.

**2026-11-14**

List and retrieve your own purchases

With a user credential, `user_id=me` on [list payments](https://docs.whop.com/api-reference/payments/list-payments.md) returns your own payments. That covers your purchases from every account and your payments on accounts you sell for. Add the `account_id` of an account you bought from to list just those purchases. [Retrieve payment](https://docs.whop.com/api-reference/payments/retrieve-payment.md) returns a payment you bought.

A purchase shows what you can see as its buyer. Account-side fields, such as `amount_after_fees`, `usd_total`, `holds` and `metadata`, are null or empty on it. `amount_charged` states what you were charged, buyer fees included.

**2026-11-13-1**

Team members carry their account as an object

Team members no longer have a top-level `account_id`. Read the account ID from `account.id`. The `account` object also has the account's `title`, `route` and `logo_url`.

**2026-11-13**

The leads API is retired

The `/leads` endpoints are deprecated and no longer listed in the API reference. At this version and later they return a `410`. Earlier versions keep working until November 20, 2026, with `Deprecation`, `Sunset` and successor `Link` headers. After that date every version returns a `410`.

Each lead is also a member of its business, with the lead's ID in the member's `metadata` as `lead_id`. Use [List members](https://docs.whop.com/api-reference/members/list-members.md) instead of listing leads. Add a member by email with [Create member](https://docs.whop.com/api-reference/members/create-member.md). Read or update one with [Retrieve member](https://docs.whop.com/api-reference/members/retrieve-member.md) and [Update member](https://docs.whop.com/api-reference/members/update-member.md).

**2026-11-12**

The referred users list is deprecated

`GET /partners/referred_users` is deprecated and no longer listed in the API reference. It keeps responding for existing integrations, with a `Deprecation` header on every response, and has no replacement.

**2026-11-11**

Recurring invoices include the upfront fee

Creating an invoice or replacing its items charges a recurring variant's `initial_price` plus `renewal_price` on the first invoice. Renewal invoices charge only `renewal_price`. Editing other fields preserves the invoice's saved prices.

Paying the first invoice starts a paid billing period immediately, without the variant's trial. A finite payment count includes that invoice: a $75 upfront fee with three $40 installments bills $115, then $40, then \$40.

**2026-11-10-1**

The renewing Economic Intelligence offer is `3_days`

TypeScript SDK: [`@whop/sdk@2.8.0`](https://unpkg.com/@whop/sdk@2.8.0/dist/esm/BaseClient.mjs).

The renewing offer in `economic_intelligence_offers` on [Retrieve account preferences](https://docs.whop.com/api-reference/accounts/retrieve-account-preferences.md) now has the key `3_days`. It renews every 3 days. To turn it on, send `economic_intelligence_duration_key: "3_days"` to [Update account preferences](https://docs.whop.com/api-reference/accounts/update-account-preferences.md). The `weekly` key is rejected.

**2026-11-10**

Ad stats no longer take `clicks`

The `clicks` field is removed from [Create ad stats](https://docs.whop.com/api-reference/ads/create-ad-stats.md). An hour's `clicks` are the ad's page views recorded by the Whop pixel, and a `clicks` value sent on any version is ignored.

**2026-11-07**

Chat API: channels, members, and messages

Build chat into your app. [Channels](https://docs.whop.com/api-reference/channels/list-channels.md) cover direct messages, group chats, community chats, and support chats. [Channel members](https://docs.whop.com/api-reference/channel-members/list-channel-members.md) add and remove people. [Messages](https://docs.whop.com/api-reference/messages/list-messages.md) can be sent, edited, pinned, reacted to, and voted on.

`/messages` changes shape from this version. A message has `channel_id` and `author` instead of `user`, and `edited` and `pinned` instead of `is_edited` and `is_pinned`. Pin with `POST /messages/{id}/pin` and `/unpin` instead of an update. [Delete message](https://docs.whop.com/api-reference/messages/delete-message.md) returns `204`. React and vote with `POST /messages/{id}/react`, `/unreact`, and `/vote`.

**2026-11-06**

The `bank_transfer` ledger line type is retired

Iron bank deposits are [ledger activity](https://docs.whop.com/api-reference/ledgers/ledger-activity) lines with `line_type` `onchain_deposit`, and their `source.sender_address` is `iron`. The `line_types` filter on financial activity and financial reports no longer accepts `bank_transfer`.

**2026-11-05-1**

Payout lists include the Whop Card

[List saved payout methods](https://docs.whop.com/api-reference/payouts/list-saved-payout-methods.md) includes the account's Whop Card. [List payouts](https://docs.whop.com/api-reference/payouts/list-payouts.md) includes the payouts that moved funds onto it. A saved payout method, and the `payout_method` of a [payout](https://docs.whop.com/api-reference/payouts/payout), has a `type`. `external` sends funds out of Whop. `whop_card` moves funds onto the account's own Whop Card. Leave out `whop_card` methods in a list where a user picks where to withdraw.

**2026-11-05**

A dispute filing in flight reports `generating_response`

A [dispute](https://docs.whop.com/api-reference/disputes/dispute) whose evidence is with the payment processor and not yet accepted reports `generating_response` instead of `under_review`. When the processor accepts it, the dispute moves to `under_review`. When the processor refuses it, the dispute returns to `needs_response`, and retrieving it gives `submission_failure_reason`. Both moves are sent as `dispute.updated`.

**2026-11-04-1**

Whop Levels: the applied level is `applied_level`

On the `level` of a [user](https://docs.whop.com/api-reference/users/user) and an [account](https://docs.whop.com/api-reference/accounts/account), `current` is now `applied_level`, and `processing_rate_percentage` is removed. Read a level's standard card processing percentage from its entry in `levels`, and an account's own rate from [Retrieve account fees](https://docs.whop.com/api-reference/accounts/retrieve-account-fees.md). An account's `level` also adds `account_level`, the level its own points reach.

**2026-11-04**

Advertising certifications are removed

Whop no longer offers advertising certifications. [Retrieve account preferences](https://docs.whop.com/api-reference/accounts/retrieve-account-preferences.md) and [Update account preferences](https://docs.whop.com/api-reference/accounts/update-account-preferences.md) no longer return `ads_certifications`, and an update no longer accepts it.

**2026-11-03-1**

Connected accounts are now subaccounts

Platform child accounts are called subaccounts. On [List payments](https://docs.whop.com/api-reference/payments/list-payments.md) and [List disputes](https://docs.whop.com/api-reference/disputes/list-disputes.md), and on their summary endpoints, `include_connected_accounts` is now `include_subaccounts`. On [Update account preferences](https://docs.whop.com/api-reference/accounts/update-account-preferences.md), `covers_connected_account_negative_balances` is now `covers_subaccount_negative_balances` in both the response and the update body. Financial-report rows that used the `connected_accounts` grouping now use `subaccounts`.

**2026-11-03**

Payout requests require a quote

[Create a payout](https://docs.whop.com/api-reference/payouts/create-payout.md) now requires `quote_token`. First call `POST /payouts/quotes`, then submit its token with the same payout details before the quote expires. Missing or invalid quotes return `400` with `invalid_payout_quote`. When retrying an uncertain payout, resend the original token and `Idempotency-Key` so the existing payout can be recovered even after its quote expires.

**2026-11-02-1**

An invoice's customer can't change after the draft is created

On [Update invoice](https://docs.whop.com/api-reference/invoices/update-invoice.md), a `member_id` that differs from the draft's current customer returns a `400`, including `null`. To bill someone else, first [void the draft](https://docs.whop.com/api-reference/invoices/void-invoice.md). Then [create a new invoice](https://docs.whop.com/api-reference/invoices/create-invoice.md). A draft saved without a customer can still take one.

**2026-11-02**

Snapchat connections are removed

[Connect an External Account](https://docs.whop.com/api-reference/external-accounts/connect-an-external-account.md) no longer takes `snapchat`, the last platform that used the `advertise` scope, so its `scopes` field is gone too. Sending `snapchat` or any `scopes` returns a `400`.

**2026-10-30**

Ad campaigns drop bid\_type

Ad campaigns no longer take or return `bid_type`. Each ad network picks its own campaign bidding, and `desired_cost_per_result` is your goal on every network: it never changes how ads bid. Sending `bid_type` returns a `400`.

**2026-10-29-8**

Advertising connections are removed

Ads run under Whop's own pages and profiles, so you no longer connect accounts for advertising. [Connect an External Account](https://docs.whop.com/api-reference/external-accounts/connect-an-external-account.md) no longer takes `meta_business`, and `tiktok` no longer takes the `advertise` scope. Sending either returns a `400`.

**2026-10-29-5**

Conversion value rules are removed

Conversion value rules are removed, and ad networks receive each conversion's original value. Listing conversion value rules is deprecated: it returns an empty list, it's no longer documented, and its responses carry a `Deprecation` header. Every other conversion value rule endpoint returns a `410`.

**2026-10-29-4**

Audiences drop engagement rules, and rule targets drop platform

Audiences can no longer be built from social engagement. Existing engagement audiences still list with `source_type` `engagement`, but they no longer return `engagement`. Sending `engagement`, or `engagement` as the `source_type`, to [Create audience](https://docs.whop.com/api-reference/audiences/create-audience.md) returns a `400`. Conversion value rule targets no longer take or return `platform`. Each target covers every network Whop runs it on, and sending `platform` on a target returns a `400`.

**2026-10-29-3**

External account partners are deprecated

Partnership ads are no longer available. Listing an external account's partners returns an empty list. Adding or removing a partner returns a `400`. These endpoints are no longer documented, and their responses carry a `Deprecation` header.

**2026-10-29-2**

Duplicates drop preserve\_engagement, and post and lead form lists are deprecated

Duplicating an ad campaign, ad group, or ad no longer takes `preserve_engagement`. Sending it to [Duplicate an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/duplicate-an-ad-campaign.md), [Duplicate an Ad Group](https://docs.whop.com/api-reference/ad-groups/duplicate-an-ad-group.md), or [Duplicate an Ad](https://docs.whop.com/api-reference/ads/duplicate-an-ad.md) returns a `400`. Listing an external account's posts or lead forms is deprecated: both return an empty list, they're no longer documented, and their responses carry a `Deprecation` header.

**2026-10-29-1**

Ads drop post\_id, and targeting search is deprecated

Ads no longer return `post_id`. Searching ad group targeting options and estimating ad group reach are deprecated: both keep working, but they're no longer documented, and their responses carry a `Deprecation` header. A conversion value rule target can leave out `platform` to cover every network Whop runs it on.

**2026-10-29**

Ads drop network-specific fields and the awareness objective

Ad campaigns, ad groups, and ads no longer return `platform`, and [Create an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/create-an-ad-campaign.md) no longer takes it. New campaigns can't use the `awareness` objective. Existing awareness campaigns still return it. Ads no longer take or return `existing_post_id`, `post_source`, `external_accounts`, or `translations`, and no longer return `post_thumbnail_url`. Sending a removed field, or `awareness` as the objective, to [Create an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/create-an-ad-campaign.md), [Create ad](https://docs.whop.com/api-reference/ads/create-an-ad.md), or [Update ad](https://docs.whop.com/api-reference/ads/update-an-ad.md) returns a `400`.

**2026-10-28-1**

Ads drop advanced delivery and creative fields

Ad campaigns no longer return `optimization_goal`. Ad groups no longer take or return `optimization_goal`, `conversion_location`, `message_apps`, `minimum_daily_spend`, `dynamic_creative`, `placements`, `frequency_cap`, or `detailed_targeting`, and Whop sets delivery from the campaign's `objective`. Ads no longer take or return `messaging_config`, `multi_advertiser_ads`, `lead_form`, `lead_form_id`, or `music`. Sending any of these to [Create ad group](https://docs.whop.com/api-reference/ad-groups/create-an-ad-group.md), [Update ad group](https://docs.whop.com/api-reference/ad-groups/update-an-ad-group.md), [Create ad](https://docs.whop.com/api-reference/ads/create-an-ad.md), or [Update ad](https://docs.whop.com/api-reference/ads/update-an-ad.md) returns a `400` naming the field.

**2026-10-28**

Account variant lists leave out legacy-invoice variants

[List variants](https://docs.whop.com/api-reference/variants/list-variants.md) with `account_id` leaves out variants generated for a legacy invoice, including one whose invoice was since deleted. To read one, [retrieve the variant](https://docs.whop.com/api-reference/variants/retrieve-variant.md) by ID. The deprecated `GET /plans` list behaves the same way.

**2026-10-27-3**

Swaps reject an amount above the wallet balance

On [Create swap](https://docs.whop.com/api-reference/swaps/create-swap.md), a decimal `amount` above the wallet's `from_token` balance returns a `400`. The error names the available balance. Earlier versions start the swap, which then fails on chain. The check is skipped when the balance can't be read. USDT isn't checked, since a USDT swap can also draw on the card balance.

**2026-10-27-2**

Every other resource names variants as variants

**Memberships, members, and waitlist entries.** On memberships and waitlist entries, `plan_id` is now `variant_id`, and on the variant embedded in a waitlist entry, `plan_type` is now `billing_type`. [Invite to a membership](https://docs.whop.com/api-reference/memberships/invite-to-a-membership.md) and [Create waitlist entry](https://docs.whop.com/api-reference/waitlist-entries/create-waitlist-entry.md) take `variant_id`, and [Approve all waitlist entries](https://docs.whop.com/api-reference/waitlist-entries/approve-all-waitlist-entries.md) takes and returns `variant_id`. [List memberships](https://docs.whop.com/api-reference/memberships/list-memberships.md) filters with `variant_id` and `variant_ids`, [List members](https://docs.whop.com/api-reference/members/list-members.md) with `variant_ids`, and [List waitlist entries](https://docs.whop.com/api-reference/waitlist-entries/list-waitlist-entries.md) with `variant_id`, instead of `plan_id` and `plan_ids`.

**Payments, invoices, and disputes.** On payments, disputes, and Resolution Center cases, `plan_id` is now `variant_id`, and on their `line_items`, `plan_id` and `plan_title` are now `variant_id` and `variant_title`. On payment quote and invoice `line_items`, `plan_id` is now `variant_id`. [List payments](https://docs.whop.com/api-reference/payments/list-payments.md) filters with `variant_ids` instead of `plan_ids`. [Create payment](https://docs.whop.com/api-reference/payments/create-payment.md) and [Create payment quote](https://docs.whop.com/api-reference/payment-quotes/create-payment-quote.md) take `variant_id` and `variant` instead of `plan_id` and `plan`, on the body and on each entry of `line_items`. Creating and updating an invoice take `variant_id` and `variant` instead of `plan_id` and `plan` on each entry of `items`. An inline `variant` takes `billing_type` instead of `plan_type`. On events, `related.plan` is now `related.variant`.

**Checkout.** On checkout configurations, `plan` is now `variant`, and its `plan_type` is now `billing_type`. [Create checkout configuration](https://docs.whop.com/api-reference/checkout-configurations/create-a-checkout-configuration.md) takes `variant_id` instead of `plan_id` and an inline `variant` (with `billing_type`) instead of `plan`, and [List checkout configurations](https://docs.whop.com/api-reference/checkout-configurations/list-checkout-configurations.md) filters with `variant_id`. Checkout session requests and responses use `items[].variant` instead of `items[].plan`.

**Promo codes and partners.** [List promo codes](https://docs.whop.com/api-reference/promo-codes/list-promo-codes.md) filters with `variant_ids` instead of `plan_ids`, and [Create promo code](https://docs.whop.com/api-reference/promo-codes/create-promo-code.md) takes `variant_ids`.

**2026-10-27-1**

Products and variants name variants as variants

On products, `default_plan` is now `default_variant`. On variants, and on the variants embedded in a product, `plan_type` is now `billing_type`. [Create variant](https://docs.whop.com/api-reference/variants/create-variant.md) takes `billing_type`, and [List variants](https://docs.whop.com/api-reference/variants/list-variants.md) and [List products](https://docs.whop.com/api-reference/products/list-products.md) filter with `billing_types` instead of `plan_types`.

Use `/variants` and the `variants` SDK namespace instead of `/plans`. The `/plans` endpoints still respond on every version, with `Deprecation` and successor `Link` headers. To preview tax, create a [payment quote](https://docs.whop.com/api-reference/payment-quotes/create-payment-quote.md) instead of calling `POST /variants/{id}/calculate_tax` or `POST /plans/{id}/calculate_tax`, which are deprecated the same way.

Variant IDs keep the `plan_` prefix, and the `plan:*` permission scopes and `plan.*` webhook event names are unchanged.

**2026-10-27**

Invitations return the pending team member

When [Create team member](https://docs.whop.com/api-reference/team-members/create-team-member.md) sends an invitation instead of adding the member, the `202` response is now the pending team member. Its `status` is `pending` and its ID starts with `ausri_`. The response no longer carries `object` or `invitation_sent`.

**2026-10-24**

Receipt-backed invoices

The native Invoices API uses `pay_` IDs and variant-backed `items`, including inline variant creation. Create saves a draft. You can update drafts, and void a draft you no longer need. The billed customer is a member. An email that doesn't belong to a Whop user creates one. `recipient_emails` are the invoice's own delivery addresses, separate from the billed member. The buyer fee is an adjustable percentage.

**2026-10-15-1**

Accounts no longer include stablecoin\_rails

Accounts no longer include `stablecoin_rails`.

**2026-10-15**

Verification document types are lowercase

On [Create verification](https://docs.whop.com/api-reference/verifications/create-verification.md), and on owners in `owners`, `document_type` takes lowercase values: `passport`, `id_card`, `drivers`, or `residence_permit`. The [identity documents](https://docs.whop.com/developer/verification/identity-documents.md) guide lists which ones each country accepts.

**2026-10-14-1**

Deposit card charge moved under methods

On [Create deposit](https://docs.whop.com/api-reference/deposits/create-deposit.md), the charge to a saved payment method moved from the top-level `card` to `methods.card`, beside `bank` and `crypto`. Its fields are unchanged, and it's still `null` unless `payment_method_id` was passed. Callers pinned to an earlier version keep the top-level `card`.

**2026-10-14**

Payment method domains are deprecated in favor of domains

The payment method domains endpoints are deprecated and no longer listed in the API reference. Add, check, and remove Apple Pay domains with the `apple_pay` capability of the [domains](https://docs.whop.com/api-reference/domains/domain) API instead:

* `GET /payment_method_domains`: `GET /domains` with `apple_pay=any`, or `apple_pay=ready` for verified domains. Filter by hostname with `domain` instead of `hostname`.
* `GET /payment_method_domains/{id}`: `GET /domains/{id}`.
* `POST /payment_method_domains`: `POST /domains` with `domain`, `account_id`, and `apple_pay: {}`. If the account already has the domain, `PATCH /domains/{id}` with `apple_pay: {}`.
* `POST /payment_method_domains/{id}/verify`: `POST /domains/{id}/check`.
* `DELETE /payment_method_domains/{id}`: `PATCH /domains/{id}` with `apple_pay: null`. `DELETE /domains/{id}` removes the whole domain, including any website on it.
* `status: verified` is `apple_pay.state: ready`. `status: pending` is `apple_pay.state: pending` while Whop registers the domain, or `action_required` until you host the association file and check the domain.

Registration runs in the background, so creating or checking a domain returns `pending`. Retrieve the domain until `apple_pay.state` is `ready` or `action_required`.

The `/payment_method_domains` endpoints still respond on every version, with `Deprecation` and successor `Link` headers. A domain keeps its ID, so `/domains/{id}` takes the ID a payment method domain returned. The domains API needs the `domain:read` and `domain:manage` permissions instead of `company:manage_checkout`.

**2026-10-13-1**

Native reviews

TypeScript SDK: [`@whop/sdk@2.5.0`](https://unpkg.com/@whop/sdk@2.5.0/dist/esm/BaseClient.mjs).

Reviews now use a native resource.

* Listing and retrieving reviews returns only published reviews. Retrieving a pending or removed review returns `404`.
* A retrieved review returns `account` instead of `company`.
* Listing reviews filters by `account_id` or `product_id`, sorts with `order` and `direction`, and returns 20 reviews per page by default. Without a filter, a user lists the reviews they wrote and an account or app API key lists its account's reviews.
* `company_id` is no longer accepted. Use `account_id`.
* `DELETE /reviews/{id}` deletes a review the signed-in user wrote and requires the `review:delete` scope.

**2026-10-13**

Accounts no longer have an industry group, and account classification is read-only

Accounts no longer include `industry_group`, and `PATCH /accounts/{id}` no longer accepts it. `business_type` and `industry_type` classify an account.

Account classification is read-only. Whop sets `business_type` and `industry_type`, and `PATCH /accounts/{id}` no longer accepts them.

**2026-10-09-2**

Apps no longer list previous hosted URLs

TypeScript SDK: [`@whop/sdk@2.4.0`](https://unpkg.com/@whop/sdk@2.4.0/dist/esm/BaseClient.mjs).

Apps no longer include `previous_hosted_urls`. An app's addresses are the domains that serve it: list them with `GET /domains`, or read `domains` on the app.

**2026-10-09-1**

Transfer profiles, amounts, and creation types

Transfers return `origin` and `destination` profiles with `name` and `logo_url`. The `amount` and `fee` money objects contain the recipient credit and all transfer fees. Responses also include available failure timestamps and a tracking URL. Unknown historical money and transition times are `null`.

Without account filters, `GET /transfers` lists transfers across the caller’s permitted accounts and personal wallet. Account-scoped credentials remain limited to their account.

`POST /transfers` requires `type`, either `balance` or `claim_link`. Requests without it return `400`. The `ledger` type is renamed `balance`, and `wallet_send` is removed. Earlier versions accept `ledger` and `wallet_send`, and create a `balance` transfer when `type` is left out.

**2026-10-09**

Generate a payment's receipt PDF

Payments no longer include `pdf_url`. To get a payment's receipt as a PDF, call `POST /payments/{id}/generate_pdf`. It returns a `url` to download the file and the `expires_at` when that link stops working.

**2026-10-08**

Pending funds can be awaiting a compliance review

TypeScript SDK: [`@whop/sdk@2.3.0`](https://unpkg.com/@whop/sdk@2.3.0/dist/esm/BaseClient.mjs).

An account's `payment_controls.undated_pending_reason` can be `compliance_review`. It means a verification the merchant already submitted is under review, so pending funds stay pending until it clears. Callers pinned earlier see `null` for this state.

**2026-10-07-2**

Economic Intelligence can renew every week

`economic_intelligence_offers` includes a `weekly` offer that renews every week until it's turned off. Each offer has `auto_renew`, and `weekly` is the recommended offer. To turn it on, send `economic_intelligence_duration_key: "weekly"` to `PATCH /accounts/{account_id}/preferences`.

`economic_intelligence_offers` lists only what the account can pick now. It's no longer `null` while Economic Intelligence is on: during a committed period it lists only `weekly`, as an upgrade, and while `weekly` is renewing it's empty. Earlier versions keep `null` while Economic Intelligence is on, leave out `weekly`, and recommend `1_day`.

**2026-10-07-1**

Saved-card payments are buyer-present by default

A saved payment method charge (`member_id` and `payment_method_id`) with a `return_url` now treats the buyer as present. This applies when you leave out `off_session`. If the issuer asks the buyer to authenticate, the payment is `requires_action` with a `next_action`. When you charge the card without the buyer, such as a renewal on your own schedule, pass `off_session: true`.

**2026-10-07**

Bill pay recipients no longer need Whop accounts

Creating a payout method with `recipient` creates a non-default recipient payout account linked to the sender's ledger. It creates no Whop user, Whop account, or recipient ledger. The recipient response contains `first_name`, `last_name`, and `country`. It no longer contains `user_id`. Earlier API versions omit recipient details when there is no Whop user. Earlier versions retain the user ID for existing recipients.

**2026-10-06-2**

App builds no longer take an AI prompt

Creating an app build no longer accepts `ai_prompt_id`.

**2026-10-06-1**

A metric's time series has its own path

Retrieve a metric's time series from `GET /stats/time_series/{metric}`. `GET /stats/{metric}` still serves it and names the successor in a `Deprecation` header.

**2026-10-06**

Social accounts are external accounts

Social accounts are now external accounts. Use `/external_accounts` and the `externalAccounts` SDK namespace. The former `/social_accounts` endpoints still respond on every version, with `Deprecation` and successor `Link` headers. External account IDs keep their `sacc_` prefix, and the `social_account:*` permission scopes are unchanged.

Ads name their accounts `external_accounts` instead of `social_accounts` in both requests and responses. Audience engagement rules name their account `external_account_id` instead of `social_account_id`. On `/external_accounts`, an Instagram account's owning page is `parent_external_account` instead of `parent_social_account`. Ad webhooks without a pinned API version receive `external_accounts`.

**2026-10-05-1**

Users no longer include social accounts

Users no longer return `social_accounts`.

**2026-10-05**

Setup intents are saved in the background

Creating a setup intent now returns it as `processing` and saves the payment method in the background. Creating a payment already works this way. To learn the outcome, poll the setup intent or pass its `client_secret` to `handleNextAction`. It ends up saved, refused, or waiting on a step from the buyer. Earlier versions keep saving it during the request.

**2026-09-29**

Prevented disputes report prevented

TypeScript SDK: [`@whop/sdk@2.1.0`](https://unpkg.com/@whop/sdk@2.1.0/dist/esm/BaseClient.mjs), [`@whop/sdk@2.2.0`](https://unpkg.com/@whop/sdk@2.2.0/dist/esm/BaseClient.mjs).

A dispute the customer was refunded for before any ruling now has `status: prevented` instead of `closed`. The refund came from the card network (Visa RDR) or an earlier refund.

**2026-09-28**

Economic Intelligence runs have their own statuses

A recommendation Whop AI is carrying out has `status: running`. When the run ends it becomes `executed`, or `incomplete` if it ended without carrying the recommendation out. Earlier versions show a running recommendation as `ready` and an incomplete one as `superseded`.

Recommendations also return `run_started_at`, `run_ended_at`, `run_by_user_id`, `result_url`, and `acknowledged_at`. To run a recommendation yourself, send `status: running` to `PATCH /economic_intelligence/{id}`, then `status: executed` (with an optional `result_url`) or `status: incomplete`. Send `status: acknowledged` to mark an executed run as seen. The recommendation stays `executed` and records `acknowledged_at`.

**2026-09-25**

Economic Intelligence durations are chosen by key

To turn on Economic Intelligence, send `economic_intelligence_duration_key` to `PATCH /accounts/{account_id}/preferences` instead of `economic_intelligence_duration_days`. Use the `key` of an entry in `economic_intelligence_offers`.

Each entry in `economic_intelligence_offers` has `key`, `duration`, and `duration_unit` instead of `duration_days`. A duration can be counted in `hours` or `days`.

**2026-09-24-1**

Ads copy is language-tagged, and Meta ads can run in other languages

An ad's `primary_texts`, `headlines`, and `descriptions` are arrays of `{ language, text }` objects on requests and responses. On an ad without translations, leave `language` out. The API rejects plain strings, so send `[{ "text": "…" }]` instead.

A Meta ad can also run in other languages. Set `translations: { source_language, automatic_languages }` to the language of the ad's own copy and the languages Meta translates into automatically. Every copy and `creatives` entry on that ad then names its `language`. The ad's own copy and creative use `source_language`, and each language you write yourself gets a `primary_texts` and a `headlines` entry, plus optional `descriptions` and `creatives` entries. `translations: null` turns the other languages off.

Earlier API versions can't update an ad that has translations. They get a 400 that names this version.

**2026-09-24**

Economic Intelligence turns on for a chosen duration

To turn on Economic Intelligence, send `economic_intelligence_duration_days` to `PATCH /accounts/{account_id}/preferences`. Pick a duration from `economic_intelligence_offers`, which lists each duration's fee. Economic Intelligence can't be changed or turned off until `economic_intelligence_ends_at`, and it turns off automatically then.

`economic_intelligence` is read-only. Sending it returns a `400` error.

**2026-09-23**

Trading access appears in permission checks

TypeScript SDK: [`@whop/sdk@2.0.0`](https://unpkg.com/@whop/sdk@2.0.0/dist/esm/BaseClient.mjs).

`GET /permissions` includes `crypto_wallet:trade` and `crypto_wallet:trade:read` when listing or checking permission actions for an account.

**2026-09-22-4**

Economic Intelligence operation descriptions

Economic Intelligence recommendations return `expected_tool_calls` as an ordered array of objects with `tool_name` and `description`. Descriptions identify the planned action and affected resource. Older recommendations have a `null` description.

**2026-09-22-3**

Partner referral links use one request type

Partner referral requests use `request_type: "link"` for links with or without rewards. Use `link` when creating or filtering links. Only authorized staff can configure rewards.

**2026-09-22-2**

Account rewards show partner reward progress

Retrieve Account returns partner reward milestones in `rewards`. Each milestone includes qualification progress and payout status for that account. The field contains an empty array when there are no matching rewards or the caller lacks balance or stats read access.

The legacy onboarding reward format is retired. Earlier API versions omit `rewards` from account responses.

**2026-09-22-1**

Setup intents become a native resource

The native Payments API now serves `POST /setup_intents`, `GET /setup_intents`, and `GET /setup_intents/{id}`. The Setup Intent object takes the shape of every other native resource.

* Related records are foreign-key ids instead of embedded objects: `account_id` (was `company`), `member_id` (was `member`), `payment_method_id` (was `payment_method`), and `checkout_configuration_id` (was `checkout_configuration`). The buyer is a `user` summary (`id`, `username`, `name`, `profile_picture`).
* `error_message` moves into `last_setup_error`, the same `{ code, message }` block Retrieve setup status returns. It stays `null` until something fails, and drops once the setup succeeds.
* `return_url`, `payment_method_type`, and `updated_at` are new. `created_at` and `updated_at` are ISO 8601 timestamps.
* `payment_instrument`, the display-shaped method the Payment object already carries, is on the Setup Intent too, so a saved card renders without a second request. Its `card` gains `exp_month` and `exp_year` on both resources. `card.brand` can be `null`: a saved card whose network the vault didn't record keeps its last four and expiry instead of losing the whole `card` object.
* Creating a setup intent answers `201 Created` (was `200`), honors `Idempotency-Key`, and requires exactly one of `confirmation_token` or `payment_method_id`. A confirmation token from another account is a `404`.
* `client_secret` comes back on create and retrieve, and only for setups created through this API. It requires a caller who may act on the setup: the account's `payment:charge` credential or the buyer's own token. List rows always carry `null`.
* `setup_intent.requires_action`, `setup_intent.succeeded`, and `setup_intent.canceled` webhooks pinned at or after this version deliver this same Setup Intent object as their `data`. Webhooks pinned earlier, and webhooks without a pin, keep the previous payload.
* `GET /setup_intents` lists with the standard `{ data, page_info }` envelope and cursor pagination. `account_id` is optional: an account API key lists its own account, and a user token lists every account it can read. `status` is a new filter, and an invalid value is a `400`. `created_before` and `created_after` work as before.

**2026-09-22**

Ads payment retries use the account endpoint

`POST /ad_campaigns/{id}/retry_payment` returns `410 Gone`. Use `POST /accounts/{id}/retry_ads_payment` with the account's `biz_` ID to queue one payment retry for all its campaigns. An accepted retry doesn't confirm payment success. Check campaign `delivery_status` and `issues` for the outcome.

**2026-09-15**

Three distinct 3D Secure policies

TypeScript SDK: [`@whop/sdk@1.1.5`](https://unpkg.com/@whop/sdk@1.1.5/dist/esm/BaseClient.mjs).

Accounts, variants, checkout configurations, and checkout sessions expose three 3D Secure choices:

* `mandate_challenge` requests a challenge before processing a supported on-session card payment.
* `mandate_if_required` mandates a challenge only when the payment processor requires it.
* `frictionless_if_required` uses the regular frictionless 3DS flow.

Payments of \$1,000 or more use at least `mandate_if_required`. Risk and authentication recovery requirements can override the preference. Explicit mandatory challenges are rejected until enabled on the platform.

Older API versions keep their existing values: new `mandate_challenge` selections remain conditional, existing mandatory preferences are preserved, and `frictionless` remains `frictionless`. Accounts on older versions represent frictionless as `null`.

**2026-09-13**

Trading transfers have distinct financial-report categories

Financial-report rows distinguish `trading_account_deposit`, `trading_account_withdrawal`, and their `_offset` entries from other on-chain transfers. Amounts and balances are unchanged.

Earlier versions keep the existing categories: `topup` for account deposits and `onchain_withdrawal` for account withdrawals. Global reports use the corresponding `onchain_deposit`, `onchain_withdrawal`, and `_offset` categories. Matching rows are combined without dropping transferred amounts.

**2026-09-11-1**

Triple Whale status renamed for white-label merchants

Account preferences `ads_triple_whale_integration.status` reports `requires_shop_domain` instead of `requires_shopify_store`. A `shop_domain` can now be set explicitly on the integration, so a connected Shopify store is no longer the only way to supply one.

**2026-09-11**

Business categories come from one table

TypeScript SDK: [`@whop/sdk@1.1.4`](https://unpkg.com/@whop/sdk@1.1.4/dist/esm/BaseClient.mjs).

Account `business_type`, `industry_group`, and `industry_type` are strings drawn from Whop's business categories table instead of fixed enums. New categories appear without a new API version. The [business types and industries glossary](https://docs.whop.com/api-reference/accounts/business-types.md) lists the current values.

* `coaching_and_courses` is now `education`, `physical_product` is now `ecommerce`, and `marketplace` is now `platform`.
* Industry groups and types were reorganized under the eight business types. `PATCH /accounts/{id}` accepts glossary slugs only. Unknown values return `400`.

Pinned callers on an earlier version still receive the previous business type names, and industry values that didn't exist before this version are returned as `null`.

**2026-09-09-1**

Account-owned experiments

Creating an experiment requires an owning `account_id`, prefixed `biz_`. Experiments also accept optional resource references and flag keys local to the account. Listing returns experiments for one account, and reading or managing them requires account permissions. Account experiments don't support team tags or provision a reporting provider.

Evaluation identifies the subject through `subject[account_id]`, `subject[anonymous_id]`, and `subject[user_id]`. Callers on older versions keep their existing evaluation identity parameters. Pass targeting `properties` as a JSON query value. Existing nested property query keys remain supported. Exposure accepts optional credentials and records their identity on the event.

**2026-09-09**

Named Whop withdrawal holds on pending funds

TypeScript SDK: [`@whop/sdk@1.1.3`](https://unpkg.com/@whop/sdk@1.1.3/dist/esm/BaseClient.mjs).

Account `payment_controls.undated_pending_reason` can be `withdrawals_disabled` when Whop has blocked withdrawals, so those pending funds can't become available.

`kyc_incomplete`, `pending_information_request`, and `null` for funds that are still clearing are unchanged.

**2026-09-06**

Engagement audience sources

`POST /audiences` supports engagement with videos, lead forms, Instagram profiles, and Facebook pages through a typed `engagement` definition.

* Explicit `source_type` values validate that the required source fields are present and conflicting source fields are absent.
* Audience responses include `engagement`, and `source_type` can be `engagement`. Engagement audiences can also be used as lookalike sources.
* `GET /social_accounts/{id}/posts` includes `video_id` and `caption` to help select videos without manually finding their platform identifiers.

**2026-09-04**

Ending an experiment requires findings

`POST /experiments/{id}/end` requires `findings` in the request body — a short explanation of what was learned and why the experiment ended the way it did.

Pinned callers on an earlier version may still omit it. A placeholder findings value is recorded on their behalf instead of a `400`.

**2026-09-02-2**

Deposit destinations are an account ID

TypeScript SDK: [`@whop/sdk@1.1.1`](https://unpkg.com/@whop/sdk@1.1.1/dist/esm/BaseClient.mjs), [`@whop/sdk@1.1.2`](https://unpkg.com/@whop/sdk@1.1.2/dist/esm/BaseClient.mjs).

`POST /deposits` takes a `destination` account ID string — `biz_…` or `user_…` — and nothing else.

* Raw wallet addresses are no longer accepted. Fund an account and read its addresses from `methods.crypto`.
* The object form of `destination` (`{ account_id }` / `{ address, network }`) is removed. Send the account ID on its own.
* The top-level `network` override is removed. It never changed the response: every deposit already returns an address for every supported network, so pick the one you want from `methods.crypto`.
* `metadata` is removed from both the request and the response. It was echoed back and never stored, so it couldn't be used to reconcile a later deposit.
* `account_id` on the response is no longer null, because every destination now names an account.

Pinned callers on an earlier version keep sending the object form and the removed inputs, and their responses still carry `metadata` — as an empty object rather than the value they sent. A wallet address is refused at every version: it never identified an account, so there is no older shape to keep serving. `metadata` sent with a `payment_method_id` is kept at every version, because it's attached to the card charge.

**2026-09-02-1**

Payments become a native resource

TypeScript SDK: [`@whop/sdk@1.1.0`](https://unpkg.com/@whop/sdk@1.1.0/dist/esm/BaseClient.mjs).

`POST /payments`, `GET /payments` and `GET /payments/{id}` are now served by the native Payments API, and the Payment object takes the shape of every other native resource.

* Related records are foreign-key ids instead of embedded objects: `account_id` (was `company`), `plan_id`, `product_id`, `membership_id`, `member_id`, `promo_code_id`, `shipment_id`, `payment_method_id`. The buyer is a `user` summary (`id`, `username`, `name`, `profile_picture`).
* Amounts are money objects (`{ amount, currency, decimals, display_decimals }`) instead of bare numbers: `total`, `subtotal`, `tax_amount`, `refunded_amount`, `tax_refunded_amount`, `amount_after_fees`, `usd_total`. `settlement_amount`, `settlement_currency` and `settlement_exchange_rate` are folded into `total` and `currency`. `refunded_amount` and `tax_refunded_amount` are stated as they settled, at the rate in force when each refund was issued, and `tax_refunded_amount` is now on list responses as well as retrieve.
* Disputes, refunds and Resolution Center cases are no longer embedded — list them from their own endpoints with `?payment_id=`. Embedded financing transactions and the application fee aren't carried over to the native shape.
* `last_payment_attempt` / `next_payment_attempt` are `last_payment_attempt_at` / `next_payment_attempt_at`. Card facts live on `payment_instrument`.
* Creating a payment takes `account_id` (was `company_id`), answers `201 Created`, honours `Idempotency-Key`, and accepts `capture: false` to place an authorization hold.
* Native payment reads are account-scoped: the credential must be able to read the account's payments (a team member's token or the account's API key). A buyer's own user token, which the pinned proxy versions accept for reading their own payment, isn't served natively yet — buyers keep working on their pinned version.
* `GET /payments/{id}/fees` rows are `{ type, origin, label, description, amount, settlement_amount, collected_at }` with money objects (were `name`/`amount`/`currency`/`type`).
* `GET /refunds` and `GET /refunds/{id}` return the native Refund: `payment_id` and `account_id` instead of an embedded payment, `amount` as a money object in the payment's settlement currency, `original_amount` in the processor's currency. The `company_id` filter is `account_id`, and sending `company_id` is a 400.
* `POST /payments/{id}/refund`, `POST /payments/{id}/retry` and `POST /payments/{id}/void` are native too, returning the same Payment object. Refund still takes an optional `partial_amount`.
* `GET /payments` lists with the standard `{ data, page_info }` envelope and cursor pagination. Filters are singular equality params (`status`, `billing_reason`, `currency`, `plan_id`, `product_id`, `membership_id`, `member_id`, `user_id`, `account_id`) instead of the proxy's plural arrays. As on the proxy, `billing_reason=subscription_cycle` also matches renewals recorded as `subscription_update`. Zero-amount payments are included, so the proxy's `include_free` is gone. An invalid `status` is a `400`, as is any proxy-only filter (`substatuses`, `updated_before`/`updated_after`, `checkout_configuration_ids`, plural arrays), rather than an unfiltered page. The `query` buyer search works as before. The `created_before`/`created_after` window covers the payment's creation time alone. The proxy filtered on paid-at where one existed. On list rows `settlement_time_at` is null — retrieve the payment for it.

**2026-09-02**

Legacy ad reports endpoint retired

The legacy `GET /ad_reports` endpoint is deprecated in favor of the native [Stats](https://docs.whop.com/api-reference/stats/stats) metrics and the ad entity endpoints. It's no longer served at this version.

* `GET /ad_reports` returns `410 Gone` with an `error.type` of `gone`. Use `GET /stats/time_series/ad_delivery` for `spend`, `impressions`, and `clicks` over time, scoped with a `source` path such as `whop:adcamp_xxx:*`. Use `GET /stats/time_series/events` for attributed conversions.
* Per-entity performance for a window is on the entity endpoints. `GET /ad_campaigns`, `GET /ad_groups`, and `GET /ads` accept `stats_from` and `stats_to` and return `spend`, `results`, and `return_on_ad_spend` on each row.
* Every response from the legacy endpoint, at any version, now carries `Deprecation` and `Link: <…/stats/ad_delivery>; rel="successor-version"` headers. Requests pinned to earlier versions, and requests without a version, keep working unchanged until a `Sunset` header announces the date it stops responding for every version.

**2026-08-31**

Financial report timestamp ranges

`GET /financial_reports` and `GET /financial_reports/breakdown` now use `from` and `to` ISO 8601 timestamps for report windows.

* `from_date` and `to_date` are renamed to `from` and `to`.
* Bare dates are no longer accepted. Include a time and offset in both timestamps.

**2026-08-25-2**

Explicit app verification filtering

`GET /apps` now uses `verified` as an optional equality filter instead of treating its absence as `verified=false` for public website lists.

* Omit `verified` to return publicly discoverable website blueprints from both verification states.
* Set `verified=true` for Whop-verified blueprints or `verified=false` for community blueprints.
* `recommended=true` filters recommended apps independently of verification status.

**2026-08-25-1**

Legacy withdrawals endpoints retired

The legacy `/withdrawals` endpoints are deprecated in favour of the native [Payouts](https://docs.whop.com/api-reference/payouts/payout) API and are no longer served at this version.

* `GET /withdrawals` and `POST /withdrawals` return `410 Gone` with an `error.type` of `gone`. Use `GET /payouts` and `POST /payouts` instead. Payout ids are the same `wdrl_` ids, so existing identifiers keep resolving.
* `GET /withdrawals/{id}` still responds at this version. Use `GET /payouts/{id}` for new work.
* Every response from the legacy endpoints, at any version, now carries `Deprecation` and `Link: <…/payouts>; rel="successor-version"` headers. A `Sunset` header will announce the date they stop responding for every version. Until then, requests pinned to earlier versions, and requests without a version, keep working unchanged.

**2026-08-25**

Ad post IDs

An ad's `post_id` now names the post the ad network serves, whichever way the ad was built.

* `post_id` returns the network's post for the ad — the one Meta created for an uploaded creative, or the post being promoted. It used to be `null` for every ad built from uploaded creatives.
* The post you point an ad at moved to `existing_post_id`, on both the response and the create/update body. `post_source` and `post_thumbnail_url` describe that field.

**2026-08-21-1**

Native Files API

TypeScript SDK: [`@whop/sdk@1.0.14`](https://unpkg.com/@whop/sdk@1.0.14/dist/esm/BaseClient.mjs).

`POST /files` and `GET /files/{id}` are served natively with a redesigned file object, and multipart uploads finish through the new `POST /files/{id}/complete`.

* File responses carry the standard envelope: `object`, `visibility`, and an ISO 8601 `created_at`. The `size` and `url` fields are `null` until the upload is `ready`.
* `GET /files/{id}` only resolves files you created — other callers receive a 404.

**2026-08-21**

Payouts status v2

TypeScript SDK: [`@whop/sdk@1.0.13`](https://unpkg.com/@whop/sdk@1.0.13/dist/esm/BaseClient.mjs).

The payout object's lifecycle vocabulary is rebuilt and its money fields become decimal strings.

* `status` speaks eight words: `requested`, `in_review`, `processing`, `completed`, `reversed`, `canceled`, `failed`, `denied`. A settled payout the provider reverses reads `reversed`, with the return code and `funds_returned_at` in `failure`.
* A new `status_detail` field carries the finest machine phase under the status word. Its values can grow without a version bump — `status` is the versioned contract.
* `amount`, `fee_amount`, `net_amount`, `markup_fee`, and `destination_amount` are decimal strings. `exchange_rate` stays a number.
* Payouts are created under their `wdrl_` id: the id `POST /payouts` returns is the id `GET /payouts` lists, and a payout's conversion request survives as `payout_request_id`. Conversion requests created before this version keep answering under their `cofr_` id.
* The idempotency key is sent only in the `Idempotency-Key` header, and the `idempotency_key` body field is rejected.

Requests pinned to earlier versions keep the previous vocabulary, float money, and body-field idempotency keys. Webhook payloads follow the subscription's `api_version_date` the same way: subscriptions pinned `2026-08-21` or later receive the new payout shape, earlier or unpinned subscriptions keep the previous one.

**2026-08-14**

Webhook envelope account\_id

The webhook envelope's `company_id` field is renamed to `account_id`.

* Webhook deliveries pinned to `2026-08-14` or later carry `account_id` in the envelope.
* Webhooks pinned to earlier versions — and webhooks without an `api_version_date` pin — keep `company_id`.

**2026-08-13**

In-transit balance breakdowns

TypeScript SDK: [`@whop/sdk@1.0.11`](https://unpkg.com/@whop/sdk@1.0.11/dist/esm/BaseClient.mjs).

Account and personal balance breakdowns now expose `in_transit` alongside `pending`.

* Add `pending` and `in_transit` to present the total amount awaiting settlement.
* Callers pinned to earlier versions continue receiving the combined amount in `pending`.

**2026-08-12**

Webhook API version input removed

The `api_version` input on `POST /webhooks` and `PATCH /webhooks/{id}` is removed.

* New webhooks always use the `v1` events and payloads. Requests passing `api_version` are rejected with a `400`.
* Pin a webhook's payload shape with `api_version_date` instead.
* You can no longer create `v2` or `v5` webhooks or switch webhooks to these versions.

**2026-08-10**

Native dispute alert endpoints

Dispute alerts are now a native REST resource and remain dual-served with the legacy proxy.

* `GET /dispute_alerts` lists an account's alerts with cursor pagination, and filters by `account_id`, `payment_id`, `type`, and a creation window.
* `type` replaces `alert_type` and names the two kinds an issuer sends: `early_fraud_warning` (Visa TC40 / Mastercard SAFE fraud reports) and `dispute_alert` (pre-dispute notices).
* `fee_charged` replaces `charge_for_alert` and reports whether Whop actually billed the account. Early fraud warnings are never billed.
* `actionable` reports whether refunding the payment can still prevent a chargeback.
* `payment` and `dispute` objects are replaced by the `payment_id` and `account_id` tags. Timestamps are ISO 8601, with `reported_at` for when the issuer filed the report.

**2026-08-05-1**

Fiat currency conversion on swaps

Fiat-pair swaps (`POST /swaps` with two fiat currencies) now support free-form currency conversion, and `amount` matches crypto swap semantics.

* `amount` is the amount of `from_token` to convert at the mid-market rate. No negative balance is required.
* Sizing a partial repayment of a negative `to_token` balance moved to the new `to_amount` field (denominated in `to_token`, capped at the debt). `amount` and `to_amount` are mutually exclusive.
* Omitting both still repays the full negative `to_token` balance.
* Callers pinned to earlier versions keep the previous behavior: their fiat `amount` is treated as the `to_token` repayment amount.

**2026-08-05**

Flat verification requirements

A verification's `requested_information` is now a flat list: one requirement per entry, one write per answer.

* Each entry names what's needed in `requirement`: a document such as `bank_statement`, or a field key such as `ssn`, with a `label` to show the user.
* Answer `file` entries with `file` (a direct upload ID). Answer `text`, `date`, `phone`, and `select` entries with `value`, and `address` entries with `address`. Nothing from the response is echoed back.
* An entry marked `multiple` takes several files in one answer, in slot order, front first for a two-sided document.
* An entry listing `options` also takes a `value`. For `select` entries, the options are the allowed answers. For identity documents, the options are the accepted ID types.
* Keys that don't apply are omitted, and rejected submissions carry structured `errors` with a stable `code` and a `reason`.
* The nested `requested_files`/`category` form shape is gone from this version. Callers pinned to earlier versions keep it, and their answers are translated automatically.

**2026-08-03**

Native promo code endpoints

Promo codes are now a complete top-level REST resource and remain dual-served with the legacy proxy.

* `GET /promo_codes` lists an account's promo codes and uses `account_id` instead of `company_id`.
* `POST /promo_codes` creates promo codes. `GET` and `DELETE /promo_codes/{id}` retrieve and archive them.
* `POST /promo_codes/{id}/activate` and `POST /promo_codes/{id}/deactivate` replace the legacy `PATCH` status write.
* List responses use cursor pagination and support status, product, variant, timestamp, and sorting filters.
* Callers pinned to earlier versions keep the legacy proxy contract unchanged.

**2026-07-31**

Richer parent accounts

Account responses now expose a richer parent account relationship for connected accounts.

* `parent_account` replaces `parent_account_id`.
* The parent account includes its `id`, `title`, `route`, and `logo_url`.

**2026-07-29-1**

Supported payout methods

Supported payout methods now have their own paginated endpoint.

* `GET /payouts/supported_methods` lists the payout methods an account or user is eligible to add.
* Supported methods use `object: "supported_payout_method"`, and their `podst_` IDs are passed as `supported_payout_method_id`.
* Saved payout methods expose `supported_payout_method`. Payouts expose `payout_method.supported_payout_method`.
* Use `country` to list supported methods for a country other than the payout account's country.
* `GET /payouts/methods` no longer accepts `include_available` or returns `available_destinations`.
* Callers pinned to earlier versions keep `destination_id`, `payout_destination`, and `payout_token`.

**2026-07-29**

Native card transactions, payout method arrival estimates

Card transactions now use native REST endpoints and remain dual-served with the legacy proxy.

* `GET /card_transactions` lists an account's card transactions, and `GET /card_transactions/{id}` retrieves one by its `citx_` id. The list also takes a `transaction_ids` filter to fetch specific transactions in one request.
* Card transactions are account-scoped: the owner is selected with `account_id`, defaulting to the account the credential belongs to.
* Filters on `transaction_ids`, `card_id`, `cardholder_id`, `status`, `created_after`, and `created_before`. Timestamp filters are ISO 8601.
* `cardholder_id` is new on the response: the user the card is assigned to.

Payout methods now carry amount-independent fee and delivery terms, and the quote no longer duplicates arrival estimates.

* Each payout method returns `fee_structure` (percentage, fixed amount, and currency) and `estimated_arrival` (per-speed timestamps) without requiring an `amount`.
* The quote's `standard` and `instant` objects no longer include `estimated_arrival`. Read it from the method's top-level `estimated_arrival` field.

**2026-07-27**

Native Resolution Center endpoints

Resolution Center cases now use native REST endpoints and remain dual-served with the legacy proxy.

* `status`, `escalated`, `outcome`, `refund`, `reason`, and `available_actions` expose case state and permitted actions.
* Events are available from paginated `GET /resolution_center_cases/{id}/events`. Summaries are available from `GET /resolution_center_cases/summary`.
* Writes use `message` and `attachments`. `due_date` is renamed `response_due_at`. Listing no longer requires `account_id`.

**2026-07-26**

Native shipments endpoints

Shipments now use native REST endpoints and remain dual-served with the legacy proxy.

* `GET /shipments` lists shipments. `GET /shipments/{id}` retrieves by shipment id or payment id.
* `POST /shipments` creates a shipment, and `PATCH /shipments/{id}` updates its tracking number.
* Responses use `account_id` and `tracking_number`, alongside `carrier`, `tracking_url`, `order_id`, and `payment_id`.
* Callers pinned before this date keep the legacy proxy contract unchanged.

**2026-07-25**

Native disputes endpoints

Disputes now use native REST endpoints and remain dual-served with the legacy proxy.

* `PATCH /disputes/{id}` edits evidence, and `POST /disputes/{id}/submit` submits it. Evidence is nested under `evidence`.
* `GET /disputes/summary` provides totals grouped by `status` and `currency`. List and retrieve return the same fields.
* Responses use `account_id`, `product_id`, and `plan_id`, plus `buyer` alongside `payment`. Listing no longer requires `account_id`.
* `status` and `reason` are normalized enums, and `needs_response_by`, `rdr`, and `editable` are replaced by their new fields.
* Callers pinned before this date keep the legacy proxy contract unchanged.

**2026-07-23**

Members and memberships

Members and memberships now use native resources with an account-oriented membership model and redesigned lifecycle actions.

* Membership responses return the compatibility field `plan_id` and `product_id` instead of nested compatibility `plan` and `product` objects.
* Listing memberships returns everything the caller can read (their own plus their managed accounts') and `account_id`/`user_id` narrow that list instead of switching modes or erroring.
* Set `cancel_at_period_end` to schedule cancellation. The `cancel` action ends access immediately.
* The `extend` action replaces `add_free_days`.

**2026-07-22**

REST response and timestamp consistency

The Current API now uses consistent delete responses, timestamp inputs, and Account naming.

* Delete endpoints for products, variants, checkout configurations, ads, ad groups, ad campaigns, social accounts, and bounty submissions return `{ id, deleted: true }` instead of a bare boolean.
* Timestamp filters on products, variants, checkout configurations, transfers, and financial reports accept ISO 8601 only instead of also accepting epoch seconds.
* Product responses return `account` instead of `company`.

**2026-07-20**

Partner business payout percentages

Partner businesses now expose separate payout rates for every income source.

* `payout_percentage` is replaced by `payout_percentages`.
* The nested object includes `sales`, `ad_spend`, `transfer`, and `card_interchange` rates.

**2026-07-18**

Products and checkout configurations

Products and checkout configurations now use the Account model consistently.

* Product request parameters use `account_id` instead of `company_id`.
* Checkout configuration requests use `account_id` at the top level and inside inline variant objects carried by the compatibility field `plan`.
* Checkout configuration responses return `account_id` instead of `company_id`.

**2026-07-08-1**

Checkout configuration timestamps

Checkout configuration timestamps now use the same format as the rest of the API.

* `created_at` and `updated_at` are ISO 8601 strings instead of Unix epoch integers.

**2026-07-08**

User balances

User balances now provide a complete, structured balance summary.

* The flat `total_usd` and `balances` fields are replaced by a nested `balance` object.
* The summary separates cash, crypto, in-flight treasury deposits, and balances for accounts the user owns.

**2026-07-01**

Business referral earnings resources

Business referral earnings now identify the polymorphic resource that generated the earning.

* `receipt` is replaced by `resource`.
* `access_pass` is replaced by `product`.
* Receipt-backed earnings return `resource.object: "receipt"` with receipt payment details.
* The `resource` field can support additional earning resources in future versions without reusing receipt-specific fields.

**2026-06-20**

Business referrals resource

Business referral volume and earnings are now reported as reconciling groups.

* `processing_volume`, `total_earnings`, `pending_payout`, and `completed_payout` are replaced by nested `volume_usd` and `earnings_usd` objects.
* Earnings rename `base_amount`/`amount` to `transaction_amount_usd`/`commission_amount_usd` and express `payout_percentage` as a fraction.

**2026-06-09**

Variants resource (then named Plans)

Variants, which were then named Plans, now use the Account model consistently.

* Request parameters and request bodies use `account_id` instead of `company_id`.
* Variant responses return `account` instead of `company`.

**2026-06-08**

Users resource

User access requests now use the Account model consistently.

* Request parameters and request bodies use `account_id` instead of `company_id`.
* Response shapes are unchanged.

**2025-01-01**

Original version

The original Current API behavior before dated versioning existed.

Requests without `Api-Version-Date` use this version so existing integrations keep working.


## Related topics

- [Migrate from Previous Versions](https://docs.whop.com/developer/api/migrate-from-previous-versions.md)
- [API Keys Reference](https://docs.whop.com/api-reference/api-keys/api-key.md)
- [Automations API](https://docs.whop.com/api-reference/automations/automation.md)
