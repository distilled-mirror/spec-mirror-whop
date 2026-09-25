> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CLI Commands

> Every Whop CLI command group and subcommand, with descriptions from the CLI itself. Captured from whop 0.16.1.

Every command group in `whop`, with each group's own description. Run `whop <group> --help` for flags, or `whop --llms-full` for the complete machine-readable reference. Commands that change state ask for confirmation before they run.

<Warning>
  The CLI has no sandbox, test, or dry-run mode, so every command runs in production.
</Warning>

## Get started

### `quickstart`

Set up the business account the CLI should use. Choose or create interactively, or pass `--account_id biz_xxx` / `--business_name`.

### `apps`

Build and deploy fully hosted web apps on Whop (`*.whop.site`).

| Command                                                   | Description                                                                  |
| --------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `whop apps init`                                          | Start a new app: register, scaffold, and link a project                      |
| `whop apps dev`                                           | Run the local dev server for this app                                        |
| `whop apps deploy`                                        | Build, upload and ship this app live (`--preview` uploads without promoting) |
| `whop apps pull`                                          | Fetch the app's deployed source and merge it into this directory             |
| `whop apps create` / `get` / `list` / `update` / `delete` | Manage app records                                                           |
| `whop apps builds get` / `list` / `promote`               | Manage app builds, promoting an older build is how you roll back             |
| `whop apps secrets list` / `set` / `unset`                | Manage app secrets, injected as environment bindings into the hosted runtime |
| `whop apps logs`                                          | List app logs                                                                |
| `whop apps permissions`                                   | Update requested permissions                                                 |

### `upgrade`

Update the CLI to the latest version. `--check` reports without installing.

## Feedback and questions

```bash theme={null}
whop report-feedback --content "The error message does not explain which permission is missing."
whop ask-question --content "How do trials interact with an existing subscription?" --format json
```

Both commands submit one observation for Whop's internal review and return a receipt. `ask-question` records an unanswered question. It doesn't return an answer or start a support conversation. Use a user credential from `whop login` (browser OAuth). Remove secrets, personal data, and payment details from `--content`.

Use `--account_id biz_...` to identify the account for an affected resource. Both commands default to your active account when available. Account context is optional.

## Commerce

### `products`

The things you sell. Each owns plans and a store page.

`create` · `get` · `list` · `update` · `delete` · `publish` · `unpublish`

`create` and `update` accept `--gallery_images` as a JSON array of objects containing an uploaded file `id` or a completed `direct_upload_id`:

```bash theme={null}
whop products update prod_xxxxxxxx --gallery_images '[{"id":"file_xxxxxxxx"}]'
```

The array replaces the gallery in display order, so include every file you want to keep. Pass `'[]'` to clear it. Omitting the option preserves the gallery. `banner_image` is separate and doesn't satisfy the marketplace gallery requirement.

### `plans`

Pricing for a product: one-time, recurring, trials, stock.

`create` · `get` · `list` · `update` · `delete` · `calculate_tax`

### `promo-codes`

Discounts that creators configure for checkout.

`create` · `get` · `list` · `delete` · `activate` · `deactivate`

### `memberships`

A customer's purchase of a plan, from checkout through cancellation.

`get` · `list` · `update` · `cancel` · `pause` · `resume` · `extend` · `transfer` · `invite`

### `checkout-configurations`

Turn a plan into a shareable, prefilled checkout link.

`create` · `get` · `list` · `delete`

### `payment-method-domains`

Domains verified to show wallet payment methods like Apple Pay at checkout.

`create` · `get` · `list` · `delete` · `verify`

### `shipments`

Track the delivery of an order by its carrier tracking number.

`create` · `get` · `list` · `update`

## Core resources

### `accounts`

A business on Whop: profile, wallet, capabilities, settings.

`create` · `get` · `me` · `list` · `update` · `form_company` · `transfer_ownership` · `preferences` · `update-preferences` · `reserves`

### `users`

A person on Whop: profile and connected identities.

`get` · `me` · `list` · `update` · `update-me` · `access` · `authorize-app` · `passkeys` · `register-passkey` · `delete-passkey` · `passkey-challenge` · `preferences` · `update-preferences` · `notification-topic-preferences` · `experience-notification-preferences` · `set-notification-preferences` · `me-oauth_grants` · `recommend_actions`

### `team-members`

An account's team members and the roles that scope their access.

`create` · `get` · `list` · `update` · `delete`

### `members`

One buyer's relationship with an account, across all their purchases.

`get` · `list` · `logs`

### `webhooks`

Event notifications pushed to your server as things happen.

`create` · `get` · `list` · `update` · `delete` · `test` · `deliveries` · `deliveries-replay` · `replay`

### `stats`

Aggregated financial, audience, and traffic reporting.

`list` (the metric catalog) · `get` (one metric, with filters, breakdowns, and intervals)

Array flags repeat (`--ad_campaign_ids adcamp_a --ad_campaign_ids adcamp_b`). Object flags take JSON, such as the ordered steps of a funnel:

```bash theme={null}
whop stats get funnel --from 2026-09-01 --to 2026-09-21 \
  --steps '{"1":{"event":"pixel.page"},"2":{"event":"payment.completed"}}'
```

### `verifications`

Legal identity that Whop requires before payouts and card issuing.

`create` · `get` · `list` · `update`

### `exports`

Asynchronous CSV dumps of an account's dashboard data.

`create` · `get` · `list`. Covers 40+ exportable resources, such as payouts, receipts, and promo codes.

## Money

### `payments`

A charge to a buyer, and the step they still owe.

`status` · `return_url`

### `setup-intents`

Saving a buyer's payment method without charging it.

`status` · `return_url`

### `ledgers`

The activity feed behind an account or user's balance.

`list` (financial activity) · `report` (balance summary, income statement, balance activity)

### `payouts`

Send money from a balance to a bank or wallet.

`create` · `get` · `list` · `methods` · `create-method` · `update-method` · `delete-method` · `supported-methods`

### Trading balances on accounts and users

Read trading balances, positions and open orders from the supported provider. These commands don't place orders or move funds. Hyperliquid is currently supported.

<Note>
  Trading is available by request. Email [support@whop.com](mailto:support@whop.com) to get access.
</Note>

```bash theme={null}
whop accounts get biz_xxx --include_trading true --format json
whop users get me --include_trading true --format json
```

### `cards`

Issue cards that spend from a balance.

`create` · `get` · `list` · `update` · `transactions` · `get-transaction`

### `transfers`

Move funds between Whop accounts and users.

`create` for a ledger transfer, wallet send, or claim link · `get` · `list` · `recipients`

### `disputes`

Chargebacks filed on an account, with the full evidence trail.

`get` · `list` · `update` (evidence) · `submit` · `summary`

### `dispute-alerts`

Issuer warnings that arrive before a chargeback does.

`get` · `list`

### `deposits`

Add funds to a balance.

`create`

### `swaps`

Convert a balance between currencies.

`create` · `get` · `quote` · `status`

### `resolution-center-cases`

File or respond to a case on a payment, whether you are the buyer or the merchant.

`create` · `get` · `list` · `reply` · `accept` · `deny` · `appeal` · `withdraw` · `request_info` · `events` · `summary`

## Ads

### `ads`

The creative: copy, assets, and destination URL.

`create` · `get` · `list` · `update` · `delete` · `duplicate` · `pause` · `unpause`

### `ad-campaigns`

Platform, objective, and budget for a set of ads.

`create` · `get` · `list` · `update` · `delete` · `duplicate` · `pause` · `unpause` · `retry_payment`

### `ad-groups`

Audience, placements, and schedule within a campaign.

`create` · `get` · `list` · `update` · `delete` · `duplicate` · `pause` · `unpause` · `estimate_reach` · `targeting_options`

### `audiences`

Reusable targeting lists for ad groups.

`create` · `list` · `update` · `delete` · `add_people`

## Experiments

### `experiments`

Create and manage experiments for an account. Pass `--account_id` to select the owning business. Account experiments don't accept or require tags. Creation currently requires Whop internal access.

`list` · `create` · `get` · `update` · `exposures` · `activate` · `pause` · `end`

## Tracking

### `people`

Visitors and customers of an account, with identity, purchase, and traffic profiles.

`get` · `list`

### `events`

Conversion and engagement events tracked for attribution.

`create` · `list` · `pulse` · `validate_pixel`

### `recommended-actions`

Suggested next-step action chains for an account.

`list` · `get` · `create` (execute a chain) · `executions`

## Workforce

### `bounties`

Paid tasks with reviewed submissions and escrowed rewards.

`create` · `get` · `list` · `update` · `cancel` · `submissions`

### `bounty-submissions`

Work submitted to a bounty, from attempt to payout.

`create` · `get` · `list` · `submit` · `delete` (cancel)

## Partners

### `partners`

The users and businesses you referred to Whop, and what you earn from them.

`create` (enroll) · `retrieve <id>` (your user ID or `me`) · `get` · `list` · `earnings` · `referred_users` · `leaderboard`

## Media

### `media`

AI-generated assets, billed from a balance. Attach them anywhere that accepts files.

`generate` (image or video, `--wait` blocks until done) · `get`

## Identity

### `social-accounts`

Connected Facebook and Instagram accounts that run ads.

`connect` · `create` · `list` · `delete` · `posts` · `lead_forms`

## Developer

### `app-builds`

Versioned build artifacts deployed to an app's platforms.

`create` · `get` · `list` · `promote`

### `api-keys`

Programmatic credentials for an account or app. The `permissions` subcommand lists the permission catalog.

`create` · `get` · `list` · `update` · `delete` · `rotate` · `permissions` (the permission catalog)

### `permissions`

What your credential can do on a resource.

`check`

### `files`

Upload local files and retrieve uploaded file details.

`create` (`--filepath` uploads a local file end to end) · `get` · `list` · `complete` (finish a multipart upload)

## Notifications

### `notifications`

The user's notification feed: unread badges, mark-read, app sends, and the topic catalog.

`create` (send) · `get` · `list` · `badges` · `mark_read` · `topics`

## Auth

### `auth`

Manage authentication.

`login` · `logout` · `status` for the active identity · `list` · `switch` · `account`, plus the top-level aliases `whop login` and `whop logout`

## Integrations

### `skills`

Sync skill files to agents.

`add` · `list`. See [Agent mode](/cli/agent-mode).

### `completions`

Generate shell completion script.

`whop completions <bash|fish|nushell|zsh>`
