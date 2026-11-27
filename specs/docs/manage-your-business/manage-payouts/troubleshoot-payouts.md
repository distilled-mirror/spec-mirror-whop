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

# Troubleshoot Payouts

> Understand payout statuses and processing delays, trace a completed payout that hasn't arrived in your bank, and withdraw your affiliate earnings.

## Common payout statuses

* **Requested:** You initiated a withdrawal, and it awaits approval
* **Processing / In Transit:** Payment is processing (takes 1–5 business days)
* **Completed:** Funds sent by Whop, awaiting bank processing
* **Canceled:** Your payout has failed

## Payout timing and processing delays

**Standard timing:**

* When you make a sale, the money will be available for withdrawal in 1–4 business days
* After a withdrawal, wait up to 10 business days for Whop and your bank to process the payout

**If a payout fails:**

* The payout timeline shows the reason
* The failed amount is automatically re-added to your available balance

**How to retry a failed payout:**

1. Review the failure reason and update any necessary information (like bank details or currency settings)
2. Once updated, select **Withdraw** in the Balances tab on your Dashboard to retry

You can also select the payout to see more information on the payout timeline and why the payout may have failed.

**Common reasons for failed payouts:**

* The bank account doesn't accept the currency of the payout
* You entered your bank account information incorrectly

**To replace your bank account:**

1. Go to your **Dashboard** > **Balances**
2. Select **Withdraw**
3. Navigate to send payout to and then **Add new account**
4. Input your new bank account details and select **Add** (bottom right)

## If a payout says "Completed" but money hasn't arrived

This means Whop completed payout processing.

1. Wait up to 5 business days (excluding weekends and bank holidays) for your bank to process it
2. If it takes longer, select the payout to see the payout timeline. The timeline provides a Trace ID

### To track or trace your payout

1. Contact your bank's customer service
2. Share the Trace ID, payout date, amount, and payer name (Whop)

## Withdraw affiliate earnings

There is usually a 14-day waiting period from when the referred customer makes their purchase and when the referrer gets paid the affiliate cut. Some commissions can wait up to 30 days. This waiting period is to protect businesses on Whop from potential disputes and refunds.

### To withdraw your affiliate earnings

1. Go to your **Profile** > **Balance**
2. Select **Withdraw**
3. Input the **Amount** you want to withdraw and select **Confirm withdrawal**


## Related topics

- [Set Up Payouts](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md)
- [Instant Withdrawals](https://docs.whop.com/manage-your-business/manage-payouts/instant-withdrawals.md)
- [Payout Methods](https://docs.whop.com/manage-your-business/manage-payouts/payout-methods.md)
