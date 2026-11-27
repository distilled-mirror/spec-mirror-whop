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

# Failed Subscription Payments

> When a subscription payment is unsuccessful, Whop, the merchant, and the customer can take steps to fix the failure.

## What happens after a payment decline

When a customer's payment gets declined or fails, Whop takes several steps to try and recover the payment:

* **The subscription status changes to Past due:** The payment needs attention, but the status doesn't immediately cancel access
  * Go to **Dashboard** > **Payments**, select **Status**, and select only **Past due**
* **Whop emails the customer:** Your customers receive emails prompting them to update their payment information
* **Whop retries the payment:** Whop automatically retries the charge over a five-day period
* **Access may be temporarily paused:** Depending on your settings, your customer might lose access to your whop until the payment goes through
  * Go to **Dashboard** > **Settings** > **Checkout**
  * Choose whether members have **Access while past due**
* **Whop cancels the subscription:** If the payment still fails after five days, Whop automatically cancels the subscription

## For merchants — how to fix a declined payment

After a payment decline, you can help the customer update their payment method and manually retry the payment from your dashboard.

1. **Ask the customer to update their payment method**
   * Contact the customer to confirm they have sufficient funds and a working payment method
   * To update their payment method, ask them to follow these steps:
     1. Go to [https://whop.com/@me/settings/memberships](https://whop.com/@me/settings/memberships)
     2. Select the inactive membership
     3. Select **Update payment method**
     4. Add the new card details and save

2. **Manually retry the payment from your dashboard**

   1. Go to **Dashboard** > **Payments**
   2. Select **Status** and select only **Failed**
   3. Select the failed payment
   4. Select **Retry payment**

   When the payment processes successfully, Whop restores the customer's access automatically.

### How to change a subscription renewal date

To adjust a customer's renewal date, the merchant can add free days to the plan. This change postpones the customer's next charge. See [how to add free days to a plan](https://docs.whop.com/manage-your-business/manage-payments/manage-users.md#how-to-add-free-days-to-a-plan).

For a subscription in the middle of a billing cycle, calculate how many days to add before the next charge. Merchants must make this change from their dashboard. Customers can't change their own membership. Please [contact the seller](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/contact-a-merchant.md) to make this request.

## For customers — how to fix a declined payment

If one of your subscription payments has failed:

1. Go to [membership settings](https://whop.com/@me/settings/memberships).
2. Sign in with your email.
3. Select the inactive membership
4. Select **Update Payment Method** and enter your new billing information.
5. Retry the payment. When it succeeds, you'll see a confirmation on the page.


## Related topics

- [Handle Payment Declines and Failed Renewals](https://docs.whop.com/payments/payment-declines.md)
- [Business Analytics Dashboard](https://docs.whop.com/manage-your-business/manage-business/analytics.md)
- [Checkout Branding](https://docs.whop.com/manage-your-business/payment-processing/checkout-branding.md)
