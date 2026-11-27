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

# Payments Overview

> Accept one-time and recurring payments, manage subscriptions, and handle refunds and disputes.

Payments is how you charge customers on Whop: one-time purchases, subscriptions, and everything around them. Whop runs the full money path: checkout, billing, payment methods, and the subscription lifecycle.

## What you can build

* **Take payments** with a shareable checkout link or the Checkout element on your own site.
* **Sell subscriptions** and manage their lifecycle: pause, resume, cancel, restore, add free days.
* **Save payment methods** to charge customers again later.
* **Handle the edge cases**: issue refunds, respond to disputes with evidence, and react to chargebacks as they happen.
* **Manage fraud** with payment rules that block, challenge, or allow checkouts on your own terms.

## Core objects

Checkout configuration, variant, product, membership, payment. Each is defined in [Core Concepts](https://docs.whop.com/developer/concepts.md).

## Where to go next

> [card] [Accept payments](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Take your first payment with a link or the Checkout element.

> [card] [Embed a checkout](https://docs.whop.com/developer/guides/embed-checkout.md)
>
> Walk through the Checkout element, the return page, and the webhook step by step.

> [card] [Build with payment elements](https://docs.whop.com/developer/guides/payment-elements.md)
>
> Design your own checkout form and confirm the charge from your server.

> [card] [Memberships](https://docs.whop.com/developer/guides/memberships.md)
>
> Manage the subscription lifecycle.

> [card] [Save payment methods](https://docs.whop.com/developer/guides/save-payment-methods.md)
>
> Charge customers again later.

> [card] [Refunds and disputes](https://docs.whop.com/developer/guides/refunds-and-disputes.md)
>
> Handle refunds, disputes, and chargebacks.

> [card] [Manage fraud](https://docs.whop.com/developer/guides/manage-fraud.md)
>
> Block, challenge, or allow checkouts with your own payment rules.


## Related topics

- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [Ads API Overview](https://docs.whop.com/api-reference/ads/overview.md)
- [Payment](https://docs.whop.com/api-reference/legacy/payments/payment.md)
