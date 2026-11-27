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

# Whop CLI Commands

> Every Whop CLI command group and subcommand, with descriptions from the CLI itself.

Every command group in `whop`, with each group's own description. Run `whop <group> --help` for flags, or `whop --llms-full` for the complete machine-readable reference. Commands that change state ask for confirmation before they run.

`whop --version` prints the installed version, and `whop --help` shows the API version the CLI speaks. `whop --update` installs the newest release, and reports `upToDate` when there is none (Homebrew installs use `brew upgrade whop`).

> [!WARNING]
>
> The CLI has no sandbox or test mode, so every command runs in production. `whop send --dry-run` is the only preview: it finds the recipient without sending.

## Get started

### `quickstart`

Set up the business account the CLI should use. Choose or create interactively, or pass `--account_id biz_xxx` / `--business_name`.

### `apps`

Build and deploy fully hosted web apps on Whop, served at `*.whop.site` and any custom domain attached to them.

| Command | Description |
| - | - |
| `whop apps init` | Start an app's code: register a new app (or take a never-deployed one with `--app`), scaffold, and link a project. `--account_id` registers it on another business, and `--template storefront` starts a complete shop |
| `whop apps dev` | Run the local dev server for this app |
| `whop apps deploy` | Build, upload and ship this app live (`--preview` uploads without promoting) |
| `whop apps pull` | Fetch the app's deployed source and merge it into this directory |
| `whop apps create` / `get` / `list` / `update` / `delete` | Manage app records |
| `whop apps builds get` / `list` / `promote` | Manage app builds, promoting an older build is how you roll back |
| `whop apps secrets list` / `set` / `unset` | Manage app secrets, injected as environment bindings into the hosted runtime |
| `whop apps logs` | List app logs |
| `whop apps categories` | List App Store categories |

## Commerce

### `products`

The things you sell. Each owns variants and a store page.

`create` · `get` · `list` · `update` · `delete` · `publish` · `unpublish`

`create` and `update` accept `--gallery_images` as a JSON array of objects containing an uploaded file `id` or a completed `direct_upload_id`:

```bash theme={null}
whop products update prod_xxxxxxxx --gallery_images '[{"id":"file_xxxxxxxx"}]'
```

The array replaces the gallery in display order, so include every file you want to keep. Pass `'[]'` to clear it. Omitting the option preserves the gallery. `banner_image` is separate and doesn't satisfy the marketplace gallery requirement.

### `waitlist-entries`

Join waitlists and manage customer signups awaiting approval.

`create` · `get` · `list` · `approve` · `approve_all` · `deny` · `cancel`

### `variants`

Purchasable configurations of a product.

`create` · `get` · `list` · `update` · `delete` · `calculate_tax`

### `promo-codes`

Discounts that creators configure for checkout.

`create` · `get` · `list` · `delete` · `activate` · `deactivate`

### `memberships`

A customer's purchase of a variant, from checkout through cancellation.

`get` · `list` · `update` · `cancel` · `pause` · `resume` · `reactivate` · `extend` · `transfer` · `invite` · `apply_promo_code` · `assign_affiliate` · `resync_access`

### `checkout-configurations`

Turn a variant into a shareable, prefilled checkout link.

`create` · `get` · `list` · `delete`

### `shipments`

Track the delivery of an order by its carrier tracking number.

`create` · `get` · `list` · `update`

## Core resources

### `accounts`

A business on Whop: profile, wallet, capabilities, settings.

`create` · `get` · `list` · `update` · `delete` · `suspend` · `form_company` · `transfer_ownership` · `preferences` · `update-preferences` · `reserves` · `retry_ads_payment`

`fees`: `get` · `update`

`financing-applications`: `list` · `get` · `create` · `update` · `submit`

Manage merchant payment-financing applications under an account. Creating an application returns the existing open application when one exists. It doesn't submit the application for review. `submit` does that after the merchant accepts the terms. Run each command with `--help` for its account and application arguments.

### `users`

A person on Whop: profile and connected identities.

`get` · `list` · `update` · `access` · `recommend_actions` · `fees`

`preferences`: `get` · `update`

`preferences notifications`: `topics` · `experiences` · `update`

### `team-members`

An account's team members and the roles that scope their access.

`create` · `get` · `list` · `delete`

### `members`

One buyer's relationship with an account, across all their purchases.

`get` · `list` · `logs`

### `economic-intelligence`

What an account should do next to grow, generated from its own data.

`list` · `update`

### `webhooks`

Event notifications pushed to your server as things happen.

`create` · `get` · `list` · `update` · `delete` · `test` · `deliveries` · `deliveries-replay` · `replay`

### `stats`

Aggregated financial, audience, and traffic reporting.

`time-series`: `list` (the metric catalog) · `get` (one metric, with filters, breakdowns, and intervals)

`reports`: `list` · `platform_trends`

Array flags repeat (`--ad_campaign_ids adcamp_a --ad_campaign_ids adcamp_b`). Object flags take JSON, such as the ordered steps of a funnel:

```bash theme={null}
whop stats time-series get funnel --from 2026-09-01 --to 2026-09-21 \
  --steps '{"1":{"event":"pixel.page"},"2":{"event":"payment.completed"}}'
```

### `verifications`

Legal identity that Whop requires before payouts and card issuing.

`create` · `get` · `list` · `update`

### `exports`

Asynchronous CSV dumps of an account's dashboard data.

`create` · `get` · `list`. Covers 40+ exportable resources, such as payouts, receipts, and promo codes.

## Notifications

### `notifications`

The user's notification feed: unread badges, mark-read, app sends, and the topic catalog.

`create` (send) · `get` · `list` · `badges` · `mark_read` · `topics`

## Payments

### `payments`

A charge to a buyer, and the step they still owe.

`create` · `get` · `list` · `update` · `capture` · `void` · `refund` · `retry` · `resume` · `fees` · `status` · `return_url`

### `payment-quotes`

A purchase priced before payment: its lines, promo code, and tax.

`create` · `get`

### `refunds`

Money returned to a buyer from a payment.

`get` · `list`

### `confirmation-tokens`

A short-lived reference to payment details collected from a buyer.

`get`

### `setup-intents`

Saving a buyer's payment method without charging it.

`create` · `get` · `list` · `status` · `return_url`

### `payment-rules`

Rules an account writes to decide its own payments.

`create` · `get` · `list` · `update` · `replace` · `delete` · `activate` · `deactivate` · `fields`

### `disputes`

Chargebacks filed on an account, with the full evidence trail.

`get` · `list` · `update` (evidence) · `upload_evidence` · `submit` · `summary`

### `dispute-alerts`

Issuer warnings that arrive before a chargeback does.

`get` · `list`

### `resolution-center-cases`

File or respond to a case on a payment, whether you are the buyer or the merchant.

`create` · `get` · `list` · `reply` · `accept` · `deny` · `appeal` · `withdraw` · `request_info` · `events` · `summary`

## Money

### `send`

Send money to a person or business on Whop from a balance.

`whop send <recipient> <amount>` takes an `@username`, an email address, a `user_`, `biz_` or `ldgr_` ID, or a name to search for. It sends from the business the CLI is using, or from another business with `--from`. Like any app, the CLI can't send from a personal balance. In a terminal it shows who will receive it and asks before sending. Scripts and agents get the exact command to run with `--yes` once the user confirms. `--dry-run` shows the recipient without sending.

### `ledgers`

The activity feed behind an account or user's balance.

`list` (financial activity) · `report` (balance summary, income statement, balance activity) · `breakdown`

### `payouts`

Send money from a balance to a bank or wallet.

`create` · `get` · `list` · `cancel` · `quotes` · `methods` · `create-method` · `update-method` · `delete-method` · `supported-methods`

### `cards`

Issue cards that spend from a balance.

`create` · `get` · `list` · `update` · `transactions` · `get-transaction`

### `cashback-rules`

Configure platform-sponsored card cashback.

`create` · `list` · `update` · `payout`

### `transfers`

Move funds between Whop accounts and users.

`create` for a balance transfer or claim link · `get` · `get-claim-link` · `claim` · `list` · `recipients`

Send from a balance with `whop transfers create --type balance --origin_id biz_xxx --destination_id user_xxx --amount 10 --currency usd --idempotency-key <key>`. Find a recipient's ID with `whop transfers recipients --origin_id biz_xxx --query @username`.

Retrieve a funded claim link by its `airdrp_` ID or public claim code with `whop transfers get-claim-link <id>`. For account IDs, the caller needs `airdrop_link:basic:read` on the funding account. For personal IDs, sign in as the account owner. For public previews, use the claim code. `whop transfers claim <code>` redeems one with the public claim code. A claim-link ID doesn't authorize redemption.

### `deposits`

Add funds to a balance.

`create`

### `swaps`

Convert a balance between currencies.

`create` · `get` · `quote` · `status`

### `trades`

Read the trading account address and its Hyperliquid websocket subscriptions, and list earlier trade submissions. These commands don't place orders or move funds. Read live positions and open orders from Hyperliquid over the websocket.

> [!NOTE]
>
> Trading account access is available by request. Email [support@whop.com](mailto:support@whop.com) to get access.

```bash theme={null}
whop accounts get biz_xxx --include_trading true --format json
whop users get me --include_trading true --format json
whop trades list --format json
whop trades get trop_xxx --format json
```

`whop trades list` and `whop trades get` return earlier submission records, not live fills.

## Affiliate program

### `product-affiliates`

Users enrolled to promote a product, including affiliates with no referrals.

`list`

## Partners

### `partner-referral-requests`

Request business or partner attribution and manage approval.

`create` · `get` · `list` · `accept` · `decline` · `cancel`

### `partners`

The users and businesses you referred to Whop, and what you earn from them.

`create` (enroll) · `retrieve <id>` (your user ID or `me`) · `get` · `list` · `earnings` · `referred_users` · `leaderboard`

## Workforce

### `bounties`

Paid tasks with reviewed submissions and escrowed rewards.

`create` · `get` · `list` · `update` · `cancel` · `submissions` · `get-submission`

### `bounty-submissions`

Work submitted to a bounty, from attempt to payout.

`create` · `get` · `list` · `submit` · `delete` (cancel)

## Tracking

### `people`

Visitors and customers of an account, with identity, purchase, and traffic profiles.

`get` · `list`

### `events`

Conversion and engagement events tracked for attribution.

`create` · `list` · `pulse` · `validate_pixel`

## Ads

### `ads`

The creative: copy, assets, and destination URL.

`create` · `get` · `list` · `update` · `delete` · `duplicate` · `pause` · `unpause` · `billing`

`whop ads billing` chooses what pays for ads: this business's Whop balance, one of your saved cards, or both, with one as the backup. In a terminal, it lists them and asks you to pick a primary and a backup. Scripts and agents get the same list and set it with `--primary` and `--backup`. Each takes `balance`, a card's `payt_` ID, or a balance's `ldgr_` ID, and `--backup` also takes `none`. Saved cards need a user login (`whop login`). To add a new card, open the `add_card_url` it returns.

### `ad-campaigns`

Platform, objective, and budget for a set of ads.

`create` · `get` · `list` · `update` · `delete` · `duplicate` · `pause` · `unpause`

### `ad-groups`

Audience, schedule, and budget within a campaign.

`create` · `get` · `list` · `update` · `delete` · `duplicate` · `pause` · `unpause`

### `audiences`

Reusable targeting lists for ad groups.

`create` · `list` · `update` · `delete` · `add_people`

## Media

### `files`

Upload local files and retrieve uploaded file details.

`create` (`--filepath` uploads a local file end to end) · `get` · `list` · `complete` (finish a multipart upload)

### `media`

AI-generated assets, billed from a balance. Attach them anywhere that accepts files.

`generate` (image or video, `--wait` blocks until done) · `get`

## Identity

### `external-accounts`

Accounts on other platforms linked to an account or user.

`connect` · `create` · `list` · `delete` · `refresh` · `posts` · `lead_forms` · `partners` · `add-partner` · `remove-partner`

### `social-accounts`

Deprecated compatibility endpoints for external accounts.

`connect` · `create` · `list` · `delete` · `refresh` · `posts` · `lead_forms` · `partners` · `add-partner` · `remove-partner`

## Developer

### `domains`

Custom domains assigned to hosted apps.

`get` · `list` · `update` · `delete`

### `api-keys`

Programmatic credentials for an account or app. The `permissions` subcommand lists the permission catalog.

`permissions` (the permission catalog)

### `api-logs`

Requests made to Whop's API with your account's API keys.

`list`

### `permissions`

What your credential can do on a resource.

`check`

### `experiments`

Create and manage experiments for an account. Pass `--account_id` to select the owning business. Account experiments don't accept tags. Experiment creation isn't available to all accounts yet.

`list` · `create` · `get` · `update` · `exposures` · `activate` · `pause` · `end`

### Feedback and questions

```bash theme={null}
whop report-feedback --content "The error message does not explain which permission is missing."
whop ask-question --content "How do trials interact with an existing subscription?" --format json
```

Both commands submit one observation for Whop's internal review and return a receipt. `ask-question` records an unanswered question. It doesn't return an answer or start a support conversation. Use a user credential from `whop login` (browser OAuth). Remove secrets, personal data, and payment details from `--content`.

Use `--account_id biz_...` to identify the account for an affected resource. Both commands default to your active account when available. Account context is optional.

## Auth

### `auth`

Manage authentication.

`login` · `logout` · `status` for the active identity · `list` · `switch` · `account`, plus the top-level aliases `whop login` and `whop logout`

The CLI saves credentials in `~/.config/whop/credentials.json`, encrypted with a key bound to the machine. MCP clients can't run these commands.

## Integrations

### `skills`

Sync skill files to agents.

`add` · `list`. See [Agent mode](https://docs.whop.com/cli/agent-mode.md).

### `mcp`

Register the CLI as an MCP server with your agent.

`add` (`--agent claude-code` targets one agent, `--no-global` installs into the project) · `doctor` (validates server startup and tool listing). See [Agent mode](https://docs.whop.com/cli/agent-mode.md).

### `completions`

Generate shell completion script.

`whop completions <bash|fish|nushell|zsh>`


## Related topics

- [Whop CLI](https://docs.whop.com/cli/overview.md)
- [Whop CLI Agent Mode](https://docs.whop.com/cli/agent-mode.md)
- [Build a Website with the CLI](https://docs.whop.com/developer/websites/quickstart.md)
