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

# Managing Dispute Rates

> The concrete steps that bring a dispute rate down, and how to handle a negative balance

A refund costs you one sale. A dispute costs you the sale, a dispute fee, and a hit to your dispute rate. Almost everything on this page follows from that.

Check your current rate on your [Payment health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md) dashboard. The **Why customers dispute** card tells you which of the steps below will move your rate the most.

## Refund more, and refund faster

This is the fastest lever you have.

* **Respond to open disputes and alerts now.** Each pre-chargeback [alert](https://docs.whop.com/manage-your-business/manage-payments/manage-disputes.md#set-up-early-dispute-alerts) you resolve is a dispute that never lands on your rate. Configure an auto-refund threshold to refund small alerts automatically.
* **Issue refunds when they're deserved.** A high dispute rate with a low refund rate means customers are choosing their bank over you. This usually happens because refunding felt slow or impossible. See [how to issue refunds](https://docs.whop.com/manage-your-business/manage-payments/issuing-refunds.md).
* **Keep enough balance on hand** to cover refunds instantly, so you never have to delay one.

## Push customers to the Resolution Center

Most disputes happen because the customer didn't know they could just ask you. The [Resolution Center](https://docs.whop.com/manage-your-business/manage-payments/resolution-center.md) lets customers open a case and get a refund or a reply directly from you instead of calling their bank. Every case resolved there is a dispute that never happens.

* Point unhappy customers to the Resolution Center in your product, your community, and your support replies.
* **Respond to cases fast.** You have 7 days before Whop steps in, but a fast response keeps customers from escalating to their bank.
* Set up [automatic responses](https://docs.whop.com/manage-your-business/manage-payments/resolution-center.md#set-up-auto-respond-for-small-refunds) to handle small refund requests instantly.

## Set clear expectations before payment

A large share of "fraudulent" and "not as described" disputes come from customers who felt surprised by the charge. Remove the surprise:

* **If it's a recurring membership, make that unmistakable.** State the rebill amount, the billing interval, and the renewal date before checkout, and make it obvious how to cancel. Customers who forget they subscribed dispute the rebill.
* **Make your storefront match what you deliver.** Don't let your ads promise more than your product delivers.
* **Never force a purchase.** Pre-checked upsells and confusing free-trial-to-charge flows generate disputes from customers who genuinely feel they never agreed.
* **Use a clear billing descriptor.** If customers don't recognize the name on their bank statement, they file a "fraudulent" dispute. You can customize your descriptor in your dashboard settings.

## Be proactive with your customers

* Reach out when something goes wrong (a delayed delivery, an outage, a change to your offer) before the customer does.
* Answer messages and refund requests fast. Silence is the number one reason a "credit not processed" dispute gets filed.
* If you ship physical goods, add tracking to every order and set honest delivery estimates.

## Turn on 3D Secure

3D Secure asks the customer's bank to verify a card payment before it goes through. It cuts the fraud reports Visa counts under its monitoring program, keeps you out of Mastercard's Excessive Fraud Merchant program, and shifts liability for fraud disputes to the card issuer. Turn it on under **Settings** → **Risk**.

## Keep strong policies on file

Clear policies both prevent disputes and help you win the ones you do get. Publish clear Terms of Service, refund, and cancellation policies, make them visible before checkout, and [upload them as legal documents](https://docs.whop.com/manage-your-business/manage-business/legal-documents.md) so Whop can include them automatically when you fight a dispute.

## Negative balances

Your balance can go negative if you have already withdrawn your funds and don't have enough left to cover a dispute or refund. You can pay off a negative balance from the [Balances](https://whop.com/dashboard/balance/) page of your dashboard.

## FAQs

> [accordion] Do refunds count toward the dispute rate?
>
> No. Refunds are the opposite of disputes. Issuing a refund removes a
> potential dispute.

> [accordion] Will pausing sales fix the dispute rate?
>
> Not right away. Disputes lag behind sales, so your rate can actually rise
> for a couple of weeks after you stop selling: you've stopped adding new
> payments, but the disputes from older ones are still coming in. The
> solutions on this page are reliable.

> [accordion] How does Whop remove controls from an account?
>
> As your dispute rate comes down and your account recovers,
> [controls](https://docs.whop.com/trust-and-safety/account-health/controls.md) ease and lift
> automatically. You don't need to contact support.


## Related topics

- [Manage Disputes](https://docs.whop.com/manage-your-business/manage-payments/manage-disputes.md)
- [Dispute Monitoring Programs](https://docs.whop.com/trust-and-safety/account-health/dispute-monitoring-programs.md)
- [Payment Health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md)
