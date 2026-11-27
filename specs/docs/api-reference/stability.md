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

# API Stability

> See whether each Whop API resource is on the Current API, the Legacy API, or both, and which one to build on.

Whop serves two API surfaces. The **Current API** is the versioned surface that new integrations should build on. The **Legacy API** predates it and stays fully supported, but new capability lands on the Current API first. Every resource is in one of three states.

## Current (Current API only)

Build on these directly. They have no Legacy counterpart.

API Keys · API Logs · Ad Campaigns · Ad Groups · Ad Pixels · Ads · Audiences · Bounty Submissions · Cashback Rules · Confirmation Tokens · Domains · Economic Intelligence · Events · Experiments · Exports · External Accounts · Feedback · Financial Activity · Media · Partner Referral Requests · Partners · Payment Method Domains · Payment Quotes · Payment Rules · People · Permissions · Product Affiliates · Swaps · Transfers · Users · Variants · Waitlist Entries

## Available on both surfaces

These exist on the Legacy API and the Current API. Build new integrations against the Current API column; existing Legacy integrations keep working.

| Legacy resource | Current API resource |
| - | - |
| [Access tokens](https://docs.whop.com/api-reference/legacy/access-tokens/access-token.md) | [Access tokens](https://docs.whop.com/api-reference/legacy/access-tokens/access-token.md) |
| Ad reports | [Stats](https://docs.whop.com/api-reference/stats/stats) |
| [App builds](https://docs.whop.com/api-reference/app-builds/app-build) | [App builds](https://docs.whop.com/api-reference/app-builds/app-build) |
| [Apps](https://docs.whop.com/api-reference/apps/app) | [Apps](https://docs.whop.com/api-reference/apps/app) |
| [Authorized users](https://docs.whop.com/api-reference/legacy/authorized-users/authorized-user.md) | [Team members](https://docs.whop.com/api-reference/team-members/team-member) |
| Bounties | [Bounties](https://docs.whop.com/api-reference/bounties/bounty) |
| [Card transactions](https://docs.whop.com/api-reference/legacy/card-transactions/card-transaction.md) | [Cards](https://docs.whop.com/api-reference/cards/card) |
| [Checkout configurations](https://docs.whop.com/api-reference/checkout-configurations/checkout-configuration) | [Checkout configurations](https://docs.whop.com/api-reference/checkout-configurations/checkout-configuration) |
| Companies | [Accounts](https://docs.whop.com/api-reference/accounts/account), same object, renamed |
| [Dispute alerts](https://docs.whop.com/api-reference/dispute-alerts/dispute-alert) | [Dispute alerts](https://docs.whop.com/api-reference/dispute-alerts/dispute-alert) |
| [Disputes](https://docs.whop.com/api-reference/disputes/dispute) | [Disputes](https://docs.whop.com/api-reference/disputes/dispute) |
| Entries | [Waitlist Entries](https://docs.whop.com/api-reference/waitlist-entries/waitlist-entry) |
| Fee markups | [Account fees](https://docs.whop.com/api-reference/accounts/account#fee-markups) |
| [Files](https://docs.whop.com/api-reference/files/file) | [Files](https://docs.whop.com/api-reference/files/list-files.md) |
| [Identity profiles](https://docs.whop.com/api-reference/legacy/identity-profiles/identity-profile.md) | [Verifications](https://docs.whop.com/api-reference/verifications/verification) |
| [Ledger accounts](https://docs.whop.com/api-reference/legacy/ledger-accounts/ledger-account.md) | [Account balances](https://docs.whop.com/api-reference/accounts/account#attributes) |
| [Members](https://docs.whop.com/api-reference/members/member) | [Members](https://docs.whop.com/api-reference/members/member) |
| [Memberships](https://docs.whop.com/api-reference/memberships/membership) | [Memberships](https://docs.whop.com/api-reference/memberships/membership) |
| Notifications | [Notifications](https://docs.whop.com/api-reference/notifications/list-notifications.md) |
| [Payment methods](https://docs.whop.com/api-reference/legacy/payment-methods/payment-method.md) | [Payment methods](https://docs.whop.com/api-reference/legacy/payment-methods/payment-method.md) |
| [Payments](https://docs.whop.com/api-reference/payments/payment) | [Payments](https://docs.whop.com/api-reference/payments/list-payments.md) |
| [Payout accounts](https://docs.whop.com/api-reference/legacy/payout-accounts/payout-account.md) | [Saved payout methods](https://docs.whop.com/api-reference/payouts/list-saved-payout-methods.md) |
| [Payout methods](https://docs.whop.com/api-reference/legacy/payout-methods/payout-method.md) | [Payouts](https://docs.whop.com/api-reference/payouts/payout) |
| [Products](https://docs.whop.com/api-reference/products/product) | [Products](https://docs.whop.com/api-reference/products/product) |
| [Promo codes](https://docs.whop.com/api-reference/promo-codes/promo-code) | [Promo codes](https://docs.whop.com/api-reference/promo-codes/list-promo-codes.md) |
| [Refunds](https://docs.whop.com/api-reference/refunds/refund) | [Refunds](https://docs.whop.com/api-reference/refunds/list-refunds.md) |
| [Resolution center cases](https://docs.whop.com/api-reference/resolution-center-cases/resolution-center-case) | [Resolution center cases](https://docs.whop.com/api-reference/resolution-center-cases/list-resolution-center-cases.md) |
| [Setup intents](https://docs.whop.com/api-reference/setup-intents/setup-intent) | [Setup intents](https://docs.whop.com/api-reference/setup-intents/setup-intent) |
| [Shipments](https://docs.whop.com/api-reference/shipments/shipment) | [Shipments](https://docs.whop.com/api-reference/shipments/list-shipments.md) |
| Stats | [Stats](https://docs.whop.com/api-reference/stats/stats) |
| Topups | [Deposits](https://docs.whop.com/api-reference/deposits/deposit) |
| [Verifications](https://docs.whop.com/api-reference/verifications/verification) | [Verifications](https://docs.whop.com/api-reference/verifications/verification) |
| [Webhooks](https://docs.whop.com/api-reference/webhooks/webhook) | [Webhooks](https://docs.whop.com/api-reference/webhooks/list-webhooks.md) |

## Legacy only

These have no Current API successor. Using them is fine, and they stay supported.

Account links · Affiliates · AI chats · Chat channels · Company token transactions · Course chapters · Course lesson interactions · Course lessons · Course students · Courses · DM channels · DM members · Experiences · Forum posts · Forums · Invoices · Leads · Messages · Reactions · Reviews · Support channels


## Related topics

- [Products API](https://docs.whop.com/api-reference/products/product.md)
- [Members API](https://docs.whop.com/api-reference/members/member.md)
- [Verifications API](https://docs.whop.com/api-reference/verifications/verification.md)
