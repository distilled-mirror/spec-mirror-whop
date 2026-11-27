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

# Cancel a Subscription

> Cancel a Whop subscription from the website or mobile app to stop it from renewing. You keep access until the end of your current billing period.

Canceling stops your subscription from renewing. You keep access until the end of the current billing period.

## On the website

1. Sign in to your Whop account at [whop.com](https://whop.com).
2. Go to your **Profile** → **[Orders](https://whop.com/@me/settings/orders/)**.
3. Select the subscription you want to cancel.
4. Select **Cancel membership**.

If you cancel your subscription, you'll receive an email confirming the cancellation.

## Mobile app

1. Tap your **profile picture**.
2. Select **Manage Orders**.
3. Tap the subscription you want to cancel.
4. Tap **Cancel membership**.

After you cancel, you'll still have access until the period you already paid for ends. Whop won't charge you again unless you resubscribe.

To get money back for the current period, you may need to [request a refund](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/request-a-refund.md) (subject to the creator's refund policy).

If you're leaving Whop entirely, cancel each membership this way first, then [delete your account](https://docs.whop.com/account-settings/account-security/delete-your-account.md).


## Related topics

- [Delete Your Account](https://docs.whop.com/account-settings/account-security/delete-your-account.md)
- [Billing Portal](https://docs.whop.com/payments-and-billing/manage-billing/billing-portal.md)
- [Failed Subscription Payments](https://docs.whop.com/payments-and-billing/payment-issues/troubleshoot-failed-payments.md)
