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

# Payment Health

> Use the Payment health dashboard to track your dispute and refund rates and see the controls on your account.

Your **Payment health** dashboard shows how disputes and refunds affect your account and payment controls. You'll find it in your dashboard under **Payments** → **Payment health**, at [whop.com/dashboard/payments/health](https://whop.com/dashboard/payments/health/).

## What a dispute rate is

A dispute (also called a chargeback) happens when a customer asks their bank to reverse a payment instead of asking you for a refund. Your dispute rate is the share of your payments that end in a dispute.

A few disputes are normal for any business. The problem is the rate. Card networks hold every merchant to a maximum dispute rate, and the line they care about is **1.5%**. Each network's program and limit is in [Dispute monitoring programs](https://docs.whop.com/trust-and-safety/account-health/dispute-monitoring-programs.md). Stay under it and you're in good standing. Go over it and there are consequences for you and for Whop.

## Your dispute rate, measured two ways

The dashboard shows your dispute rate in two modes: **30d window** and **By purchase date**. They answer different questions.

* **30d window**: disputes filed in the last 30 days ÷ payments taken in the last 30 days.
* **By purchase date**: takes the payments from one period and asks how many of them got disputed. Disputes filed within N days of the payment ÷ payments taken in the period, where N is the **Disputed within 7 days**, **Disputed within 14 days**, or **Disputed within 28 days** selector. The dashboard assigns each dispute to the payment date, not the filing date.

For example, say you took 1,000 payments in the last 30 days, and 10 disputes arrived in that time:

3 on payments from the last 30 days
7 on older payments

The **30d window** rate counts all 10 disputes: 10 ÷ 1,000 = **1%**. The **By purchase date** rate counts only the 3 that belong to this month's payments: 3 ÷ 1,000 = **0.3%** so far. That rate keeps rising as more disputes on this month's payments come in.

Which one to watch depends on your situation:

* **If your sales are steady**, the two rates look about the same.
* **If you're growing fast**, the 30d window rate looks low because customers haven't had time to dispute your newest payments yet. Watch the by-purchase-date rate instead: it shows your dispute rate before the 30d number catches up.

> [!NOTE]
>
> The by-purchase-date view only counts payments after their full dispute
> window has elapsed. If your business took a payment yesterday, its 28-day
> window hasn't elapsed, so the view can't score it yet.

## Statuses

Each dispute rate metric gets a status: **Healthy**, **At risk**, or **Critical**. The chart draws the exact bands for each metric, and every payment method tab has its own: American Express, Discover, and buy now, pay later methods are held to different lines than standard cards. [Dispute monitoring programs](https://docs.whop.com/trust-and-safety/account-health/dispute-monitoring-programs.md) lists every line. If your account doesn't have enough payments or disputes to score yet, the metric shows **Low data** instead.

## The controls on your account

The **Controls** section of the page lists the [payment controls](https://docs.whop.com/trust-and-safety/account-health/controls.md) currently set on your account: your reserve, auto-refund thresholds, pending delay, fees, and financing availability. Reviews run continuously, and controls ease or lift automatically as your account recovers.

## Refund rate

The dashboard also shows your refund rate: refunds issued as a share of payments. Read it next to your dispute rate. A low refund rate with a high dispute rate means customers who wanted their money back went to their bank instead of coming to you.

## Why customers dispute

The **Why customers dispute** card breaks your disputes down by reason. Each reason points at a fix:

| Reason | What the customer is saying | The fix |
| - | - | - |
| **Fraudulent** | "I don't recognize this charge" | Use a clear billing descriptor and disclose rebills up front |
| **Not received** | "My order never arrived" | Add tracking and set honest delivery estimates |
| **Subscription canceled** | "I canceled but was still charged" | Make canceling easy and honor it quickly |
| **Product unacceptable** | "This wasn't what I was promised" | Make your storefront match what you actually deliver |
| **Credit not processed** | "I was promised a refund and never got it" | Issue owed refunds fast |

> [!TIP]
>
> If one reason dominates the chart, start there. Fixing the largest slice
> moves your rate the most.

## Next steps

> [card] [Dispute monitoring programs](https://docs.whop.com/trust-and-safety/account-health/dispute-monitoring-programs.md)
>
> Each card network's program, how it counts disputes, and its limit.

> [card] [Payment controls](https://docs.whop.com/trust-and-safety/account-health/controls.md)
>
> What each control on your account does and how controls lift.

> [card] [Managing dispute rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md)
>
> The concrete steps that bring a dispute rate down.


## Related topics

- [Payment Controls](https://docs.whop.com/trust-and-safety/account-health/controls.md)
- [Reserves](https://docs.whop.com/trust-and-safety/account-health/reserves.md)
- [Dispute Monitoring Programs](https://docs.whop.com/trust-and-safety/account-health/dispute-monitoring-programs.md)
