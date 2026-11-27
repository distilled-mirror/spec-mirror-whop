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

# Whop API Overview

> How requests to the Whop API work, with a map of every resource and where it's documented.

The Current API is how you move money programmatically. Accept payments with checkout links, hold balances, send payouts, issue cards, transfer funds between accounts, and manage the products and variants you sell. Do it all from your server with one API key.

## Make a request

The base URL is `https://api.whop.com/api/v1`. Authenticate every request with a Bearer API key:

```bash theme={null}
curl https://api.whop.com/api/v1/accounts/me \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

```json theme={null}
{
	"id": "biz_XXXXXXXX",
	"title": "Shine Time Auto Detailing",
	"route": "shine-time-auto-detailing",
	"status": "approved"
}
```

If you're new, the [Quickstart](https://docs.whop.com/developer/quickstart.md) takes you from [creating a key](https://whop.com/dashboard/developer) to a live checkout link in about five minutes. If you're moving off API v2, public GraphQL, or the payment method domains API, start with [Migrate from previous versions](https://docs.whop.com/developer/api/migrate-from-previous-versions.md).

## How requests work

### Authentication

[Create an API key here](https://whop.com/dashboard/developer) under **Account API Keys**. Keys belong to an account and carry the permissions you grant them. Keep keys on your server, never in browser code, mobile apps, or public repos.

### Versioning

Whop versions the API by date. Send an `Api-Version-Date` header, such as `2026-07-01`, to pin the shapes of your requests and responses. Later changes won't break a pinned caller. When you omit the header, requests use the original `2025-01-01` shapes. Generated SDKs always send the latest version available when Whop built them. See [Versioning](https://docs.whop.com/developer/api/versioning.md).

### Pagination

List endpoints return a `data` array plus a `page_info` object. Pass `first` to choose the page size. Pass `after` with the previous response's `page_info.end_cursor` to fetch the next page. `page_info.has_next_page` tells you when to stop.

Lists that support it take `include_total_count=true` and return the number of matching results in `page_info.total_count`. Counts stop at 10,000 (`total_count_capped: true`) and are `null` if counting fails. Add `first=0` to get only the count.

### Errors

Failed requests return a conventional HTTP status (`400`, `401`, `403`, `404`) and a body with a single `error` object: `type` is a machine-readable code and `message` explains what went wrong.

```json theme={null}
{
	"error": {
		"type": "invalid_parameters",
		"message": "initial_price must be greater than 0"
	}
}
```

## Where everything lives

This reference covers the Current API. These resources are documented in the [Legacy API reference](https://docs.whop.com/api-reference/payments/payment):

* Charging: [Payments](https://docs.whop.com/api-reference/payments/payment), [Refunds](https://docs.whop.com/api-reference/refunds/refund), [Invoices](https://docs.whop.com/api-reference/legacy/invoices/legacy-invoice.md), [Payment methods](https://docs.whop.com/api-reference/legacy/payment-methods/payment-method.md)
* Content and community: [Courses](https://docs.whop.com/api-reference/legacy/courses/course.md), [Forums](https://docs.whop.com/api-reference/legacy/forums/forum.md), [Messages](https://docs.whop.com/api-reference/legacy/messages/message.md)
* Growth: [Affiliates](https://docs.whop.com/api-reference/legacy/affiliates/affiliate.md), [Reviews](https://docs.whop.com/api-reference/legacy/reviews/review.md)

Use the API selector at the top of the sidebar to switch between references.

## Resources

### Core Resources

The accounts and people everything else hangs off.

| Resource | What it's for |
| - | - |
| [Accounts](https://docs.whop.com/api-reference/accounts/account) | A business on Whop: profile, wallet, capabilities, settings. |
| [Users](https://docs.whop.com/api-reference/users/user) | A person on Whop: profile and connected identities. |
| [Members](https://docs.whop.com/api-reference/members/member) | One buyer's relationship with an account, across all their purchases. |
| [Team Members](https://docs.whop.com/api-reference/team-members/team-member) | An account's team members and the roles that scope their access. |
| [Search](https://docs.whop.com/api-reference/search/search) | Find an account's members, payments and other records in one call, by ID, email, card, phone, number, link or text. |
| [External Accounts](https://docs.whop.com/api-reference/external-accounts/external-account) | Accounts on other platforms linked to an account or user. |
| [Social Accounts](https://docs.whop.com/api-reference/social-accounts/social-account) | Deprecated compatibility endpoints for external accounts. |

### Payments

Accept payments from buyers and handle what comes after.

| Resource | What it's for |
| - | - |
| [Payments](https://docs.whop.com/api-reference/payments/payment) | A charge against a buyer, and the step they still owe. |
| [Invoices](https://docs.whop.com/api-reference/invoices/invoice) | A payment request that keeps the same receipt from draft through collection. |
| [Confirmation Tokens](https://docs.whop.com/api-reference/confirmation-tokens/confirmation-token) | A short-lived reference to payment details collected from a buyer. |
| [Payment Quotes](https://docs.whop.com/api-reference/payment-quotes/payment-quote) | A purchase priced before it is paid: its lines, promo code and tax. |
| [Setup Intents](https://docs.whop.com/api-reference/setup-intents/setup-intent) | Saving a buyer's payment method without charging it. |
| [Refunds](https://docs.whop.com/api-reference/refunds/refund) | Money returned to a buyer from a payment. |
| [Resolution Center Cases](https://docs.whop.com/api-reference/resolution-center-cases/resolution-center-case) | File or respond to a case against a payment, as the buyer or the merchant. |
| [Dispute Alerts](https://docs.whop.com/api-reference/dispute-alerts/dispute-alert) | Issuer warnings that arrive before a chargeback does. |
| [Disputes](https://docs.whop.com/api-reference/disputes/dispute) | Chargebacks filed against an account, with evidence and outcomes. |
| [Payment Rules](https://docs.whop.com/api-reference/payment-rules/payment-rule) | Rules an account writes to decide its own payments. |
| [Financing Applications](https://docs.whop.com/api-reference/financing-applications/financing-application) | An account's request to accept buy-now-pay-later payment methods. |

### Money

Balances and banking: every way funds move in or out.

| Resource | What it's for |
| - | - |
| [Deposits](https://docs.whop.com/api-reference/deposits/deposit) | Add funds to a balance. |
| [Transfers](https://docs.whop.com/api-reference/transfers/transfer) | Move funds between Whop accounts and users. |
| [Payouts](https://docs.whop.com/api-reference/payouts/payout) | Send money from a balance to a bank or wallet. |
| [Financial Activity](https://docs.whop.com/api-reference/ledgers/ledger-activity) | The activity feed behind an account or user's balance. |
| [Cards](https://docs.whop.com/api-reference/cards/card) | Issue cards that spend from a balance. |
| [Cashback Rules](https://docs.whop.com/api-reference/cashback-rules/cashback-rule) | Configure platform-sponsored card cashback. |
| [Swaps](https://docs.whop.com/api-reference/swaps/swap) | Convert a balance between currencies. |
| [Trades](https://docs.whop.com/api-reference/trades/trade) | Place orders, close positions, and move funds for perpetual and spot trading from a Whop-managed wallet. |
| [Verifications](https://docs.whop.com/api-reference/verifications/verification) | Legal identity required before payouts and card issuing. |

### Commerce

Sell products and get paid.

| Resource | What it's for |
| - | - |
| [Products](https://docs.whop.com/api-reference/products/product) | The things you sell. Each owns variants and a store page. |
| [Variants](https://docs.whop.com/api-reference/variants/variant) | Purchasable configurations of a product. |
| [Promo Codes](https://docs.whop.com/api-reference/promo-codes/promo-code) | Discounts that creators configure for checkout. |
| [Checkout Configurations](https://docs.whop.com/api-reference/checkout-configurations/checkout-configuration) | Turn a variant into a shareable, prefilled checkout link. |
| [Waitlist Entries](https://docs.whop.com/api-reference/waitlist-entries/waitlist-entry) | Join waitlists and manage customer signups awaiting approval. |
| [Memberships](https://docs.whop.com/api-reference/memberships/membership) | A customer's purchase of a variant, from checkout through cancellation. |
| [Shipments](https://docs.whop.com/api-reference/shipments/shipment) | Track the delivery of an order by its carrier tracking number. |
| [Reviews](https://docs.whop.com/api-reference/reviews/review) | Customer star ratings and written feedback on a product. |
| [Review Reports](https://docs.whop.com/api-reference/review-reports/review-report) | Reports an account's admins filed against customer reviews. |
| [Product Affiliates](https://docs.whop.com/api-reference/product-affiliates/product-affiliate) | Users enrolled to promote a product, including affiliates with no referrals. |

### Websites

Hosted apps, their builds and domains, and the experiments that run on them.

| Resource | What it's for |
| - | - |
| [Domains](https://docs.whop.com/api-reference/domains/domain) | Custom domains assigned to hosted apps. |
| [Apps](https://docs.whop.com/api-reference/apps/app) | Apps you build on Whop: metadata, hosted builds, runtime logs. |
| [App Builds](https://docs.whop.com/api-reference/app-builds/app-build) | Versioned build artifacts deployed to an app's platforms. |
| [App Installations](https://docs.whop.com/api-reference/app-installations/app-installation) | Your app's installations on accounts, and the data it keeps about each one. |
| [Experiments](https://docs.whop.com/api-reference/experiments/experiment) | Feature flags and A/B experiments for gradual rollout. |

### Ads

Run and measure ad campaigns from your integration.

| Resource | What it's for |
| - | - |
| [Ad Campaigns](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign) | Objective and budget for a set of ads. |
| [Ad Groups](https://docs.whop.com/api-reference/ad-groups/ad-group) | Audience, schedule, and budget within a campaign. |
| [Ads](https://docs.whop.com/api-reference/ads/ad) | The creative: copy, assets, and destination URL. |
| [Audiences](https://docs.whop.com/api-reference/audiences/audience) | Reusable targeting lists for ad groups. |

### Analytics

Who visits, what converts, and how an account is performing.

| Resource | What it's for |
| - | - |
| [Stats](https://docs.whop.com/api-reference/stats/stats) | Aggregated financial, audience, and traffic reporting. |
| [People](https://docs.whop.com/api-reference/people/person) | Visitors and customers of an account, with identity, purchase, and traffic profiles. |
| [Events](https://docs.whop.com/api-reference/events/event) | Conversion and engagement events tracked for attribution. |
| [Exports](https://docs.whop.com/api-reference/exports/export) | Asynchronous CSV dumps of an account's dashboard data. |
| [Recommended Actions](https://docs.whop.com/api-reference/recommended-actions/recommended-action) | Changes Whop recommends for an account, which its owner runs or dismisses. |

### Communication

Channels, the messages exchanged within them, livestreams, the notifications users receive, and email mailboxes.

| Resource | What it's for |
| - | - |
| [Channels](https://docs.whop.com/api-reference/channels/channel) | Private, community and support chat channels. |
| [Channel Members](https://docs.whop.com/api-reference/channel-members/channel-member) | Channel participants and personal membership state. |
| [Messages](https://docs.whop.com/api-reference/messages/message) | Chat messages, attachments, reactions and polls. |
| [Livestreams](https://docs.whop.com/api-reference/livestreams/livestream) | Live video broadcasts in an experience, and their recordings. |
| [Notifications](https://docs.whop.com/api-reference/notifications/notification) | The user's notification feed: unread badges, mark-read, app sends, and the topic catalog. |
| [Email Addresses](https://docs.whop.com/api-reference/email-addresses/email-address) | Mailboxes on an account's domains, for sending and receiving email. |
| [Automations](https://docs.whop.com/api-reference/automations/automation) | Workflows that a webhook event or a trigger request starts for each member, running timed steps that message them. |
| [Automation Steps](https://docs.whop.com/api-reference/automation-steps/automation-step) | The wait, email and message steps of an automation's versions. |
| [Automation Enrollments](https://docs.whop.com/api-reference/automation-enrollments/automation-enrollment) | The members going through an automation, and the step each one is waiting on. |

### Partners

Refer users and businesses to Whop and track what you earn.

| Resource | What it's for |
| - | - |
| [Partners](https://docs.whop.com/api-reference/partners/partner) | Your partner profile, referral links, payout rates, and referred businesses. |
| [Partner Referral Requests](https://docs.whop.com/api-reference/partner-referral-requests/partner-referral-request) | Request business or partner attribution and manage approval. |

### Workforce

Post paid tasks and pay people for completed work.

| Resource | What it's for |
| - | - |
| [Bounties](https://docs.whop.com/api-reference/bounties/bounty) | Paid tasks with reviewed submissions and escrowed rewards. |
| [Bounty Submissions](https://docs.whop.com/api-reference/bounty-submissions/bounty-submission) | Work submitted to a bounty, from attempt to payout. |

### Media

Generate media assets from your integration.

| Resource | What it's for |
| - | - |
| [Files](https://docs.whop.com/api-reference/files/file) | Upload files and attach them wherever Whop accepts documents. |
| [Media](https://docs.whop.com/api-reference/media/media) | AI-generated assets, billed from a balance, attachable wherever files are accepted. |

### Developer

Credentials, webhooks, and logs for integrating with the Whop API.

| Resource | What it's for |
| - | - |
| [API Keys](https://docs.whop.com/api-reference/api-keys/api-key) | Programmatic credentials for an account or app. |
| [Webhooks](https://docs.whop.com/api-reference/webhooks/webhook) | Event notifications pushed to your server as things happen. |
| [Permissions](https://docs.whop.com/api-reference/permissions/permission) | What your credential is allowed to do on a resource. |
| [API Logs](https://docs.whop.com/api-reference/api-logs/api-log) | Requests made to Whop's API with your account's API keys. |
| [Feedback](https://docs.whop.com/api-reference/feedback/feedback) | Issues and unanswered questions sent to Whop for review. |


## Related topics

- [Ads API Overview](https://docs.whop.com/api-reference/ads/overview.md)
- [Whop API Quickstart](https://docs.whop.com/developer/quickstart.md)
- [Whop API Core Concepts](https://docs.whop.com/developer/concepts.md)
