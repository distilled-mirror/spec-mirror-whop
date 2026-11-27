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

# Platform Map

> Find every Whop capability in one table, and see whether to use the API, a prebuilt element, or a hosted page.

Whop is one API with many products behind it. Each row below is a capability. Each column is a way to integrate it: call the API directly, drop in a prebuilt element, or send people to a page Whop hosts. Start from the guide in the last column.

| Capability | API + SDK | Elements | Hosted | Guide |
| - | - | - | - | - |
| **[Payments & checkout](https://docs.whop.com/developer/payments/overview.md)** | [Payments API](https://docs.whop.com/api-reference/overview.md) | [Checkout](https://docs.whop.com/elements/latest/checkout/overview), [card fields](https://docs.whop.com/elements/latest/payments/overview) | Checkout links | [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md) |
| **Subscriptions & memberships** | [Memberships](https://docs.whop.com/api-reference/memberships/membership), [Variants](https://docs.whop.com/api-reference/variants/variant) | - | - | [Memberships](https://docs.whop.com/developer/guides/memberships.md) |
| **[Payouts & money movement](https://docs.whop.com/developer/payouts/overview.md)** | [Payouts](https://docs.whop.com/api-reference/payouts/payout), [Transfers](https://docs.whop.com/api-reference/transfers/transfer), [Ledgers](https://docs.whop.com/api-reference/ledgers/ledger-activity) | [Payout portal](https://docs.whop.com/developer/platforms/render-payout-portal.md) | - | [Send payouts](https://docs.whop.com/developer/platforms/manual-payouts.md) |
| **Wallets, cards & swaps** | [Cards](https://docs.whop.com/api-reference/cards/card), [Swaps](https://docs.whop.com/api-reference/swaps/swap), [Deposits](https://docs.whop.com/api-reference/deposits/deposit) | [Wallet](https://docs.whop.com/elements/latest/wallet/overview) | - | - |
| **Identity & verification** | [Verifications](https://docs.whop.com/api-reference/verifications/verification) | [KYC element](https://docs.whop.com/elements/latest/verifications/kyc.md) | - | [Verification](https://docs.whop.com/developer/verification/overview.md) |
| **[Marketplaces & subaccounts](https://docs.whop.com/developer/marketplaces/overview.md)** | [Accounts](https://docs.whop.com/api-reference/accounts/account) | - | onboarding flows | [Platforms quickstart](https://docs.whop.com/developer/platforms/quickstart.md) |
| **[Chat, DMs & forums](https://docs.whop.com/developer/communities/overview.md)** | Chat API | [Chat](https://docs.whop.com/developer/guides/chat/chat-element.md), [DMs](https://docs.whop.com/developer/guides/chat/dms-list-element.md) | - | [Chat quickstart](https://docs.whop.com/developer/guides/chat/quickstart.md) |
| **[Apps on Whop](https://docs.whop.com/developer/apps/overview.md)** | [Apps](https://docs.whop.com/api-reference/apps/app), [Permissions](https://docs.whop.com/api-reference/permissions/permission) | [Frosted UI](https://docs.whop.com/developer/guides/frosted_ui.md) | Whop marketplace | [App views](https://docs.whop.com/developer/guides/app-views.md) |
| **[Websites](https://docs.whop.com/developer/websites/overview.md)** | [Apps](https://docs.whop.com/api-reference/apps/app), [Domains](https://docs.whop.com/api-reference/domains/domain) | - | `whop.site` hosting | [Websites quickstart](https://docs.whop.com/developer/websites/quickstart.md) |
| **[Ads & attribution](https://docs.whop.com/developer/ads/overview.md)** | [Ad campaigns](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign) | [Ads](https://docs.whop.com/elements/latest/ads/overview), [Tracking](https://docs.whop.com/elements/latest/tracking/overview) | - | [Pixel](https://docs.whop.com/developer/ads/pixel.md) |
| **Data & analytics** | [Stats](https://docs.whop.com/api-reference/stats/stats) | - | - | [Stats](https://docs.whop.com/developer/guides/stats.md) |
| **[Affiliates & partners](https://docs.whop.com/developer/partners/overview.md)** | [Partners](https://docs.whop.com/api-reference/partners/partner) | - | Affiliate links | [Affiliates](https://docs.whop.com/developer/guides/affiliates.md) |

Payments, checkout, and webhooks run in the [sandbox](https://docs.whop.com/developer/guides/sandbox.md) without moving real money. Payouts, apps, and messaging aren't available there yet. Webhooks fire for the events that matter, like `payment.succeeded` and `membership.activated`, so your server always knows what happened.

## Next steps

> [card] [Quickstart](https://docs.whop.com/developer/quickstart.md)
>
> Make your first call and take a test payment in the sandbox.

> [card] [Core Concepts](https://docs.whop.com/developer/concepts.md)
>
> The objects behind every row, and how they connect.


## Related topics

- [Enroll Subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md)
- [Marketplaces & Platforms](https://docs.whop.com/developer/marketplaces/overview.md)
- [Platform Quickstart](https://docs.whop.com/developer/platforms/quickstart.md)
