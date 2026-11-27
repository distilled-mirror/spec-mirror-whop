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

# Issue Refunds

> Refund a customer or set up automatic refunds from your Whop dashboard, including what to do when your balance runs short.

Process refunds instantly, set up automatic refunds, and manage your account balance on Whop.

## How to process a refund

There are three ways to refund a customer: through their **profile** (quickest method), **Payments**, or the **Resolution Center**.

> [tab] Customer's profile
>
> 1. Go to your **Dashboard**
> 2. Use the search bar at the top right to find the customer
> 3. Select their profile
> 4. Scroll down to **Payments** and select **Refund** next to the specific payment
> 5. Choose your refund amount:
>    * Select **Refund** for the full amount
>    * Check **Partial refund** and enter the amount you want to refund

> [tab] Payments
>
> Great if you want to see all your transactions:
>
> 1. Go to **Dashboard** > **Payments**
> 2. Find the payment you want to refund
> 3. Select the 3 dots (⋮) next to the customer
> 4. Select **Refund** from the dropdown
> 5. Choose your refund amount:
>    * Select **Refund** for the full amount
>    * Check **Partial refund** and enter the amount you want to refund

> [tab] Resolution Center
>
> 1. Go to **Dashboard** > **Resolution Center**
> 2. Select the case you want to resolve
> 3. Select **Accept** to process the refund

> [!TIP]
>
> Whop processes refunds immediately, and you'll see the refund appear on the
> customer's profile right away.

## Set up automatic refunds to save time

If you want to automatically handle smaller refund requests, you can set up automatic refunds for amounts under a certain threshold.

To set up automatic refunds:

1. Go to **Dashboard** > **Resolution Center**
2. Select **Auto respond**
3. Choose your automatic refund thresholds:
   * **Auto-refund card payments below**: Choose the maximum amount for automatic card refunds
   * **Auto-refund PayPal payments below**: Choose the maximum amount for automatic PayPal refunds
4. Add a custom message (optional) that customers will see when they receive an automatic refund
5. Select **Save**

> [!NOTE]
>
> Resolution Center cases for purchases made with financing options are always
> **automatically refunded.** This applies to Splitit, Sezzle, Klarna, Afterpay,
> Zip Pay, Scalapay, SeQura, and ClarityPay. Automatic refunds help protect
> your payments, keep your dispute rates low, and preserve your access to BNPL
> financing options. Whop handles these refunds automatically, so you don't
> need to act.
>
> ClarityPay payments are an exception to the standard refund timeline.
> Customers can submit refund requests for up to 365 days after the original
> payment. However, automatic refunds only apply to cases submitted within the
> first 120 days. Cases submitted after this period aren't automatically
> refunded. They follow the standard Resolution Center process, and you
> review and resolve each case as you would any other.

![Auto refunds](https://mintcdn.com/whop/gkU9N_oxv5Gb4qnv/images/autorefundsetup.png?fit=max\&auto=format\&n=gkU9N_oxv5Gb4qnv\&q=85\&s=d115686b01deeffb2b4170913db2c473)

## What to do if you don't have sufficient funds

If your Whop account doesn't have sufficient funds for a refund, you'll need to add money to your account first.

### Top up your balance

To add funds to your account:

1. Go to **Dashboard** > **Balances**
2. Select **Top up +**
3. Enter the amount you want to add
4. Choose to pay from your personal balance or select another payment method
5. Select **Pay**

> [!TIP]
>
> Always keep some money in your account so you can process refunds when needed.
> This keeps customers happy and makes your business look professional.

### Set up minimum balance retention

If you use **Automatic withdrawals**, you can automatically keep money in your account for refunds:

1. Go to **Dashboard** > **Balances**
2. Select **Automatic withdrawals**
3. Choose your withdrawal schedule (**Daily**, **Weekly**, or **Monthly**) and select **Continue**
4. Enter the minimum amount you want to keep in your account and select **Continue**
   * Consider keeping enough to cover at least two refunds at your typical price
5. Select **Confirm**

To edit an existing minimum amount, select the 3 dots at the top right of your **Payouts** page. Select **Automatic withdrawals**, and then select **Edit automatic withdrawal settings**.

> [!TIP]
>
> If you only use manual withdrawals, you can't choose a minimum balance to retain
> — you'll need to manage your balance manually.

## Frequently asked questions

> [accordion] How do fees work with refunds?
>
> When you refund someone, you pay back the full amount they originally paid, even though fees reduced the amount you received.
>
> For example: If a customer paid \$100, you'll refund the full \$100 even though you only received around \$97 after Whop and processing fees.
>
> This applies to both full and partial refunds. There are no additional fees to process a refund.

> [accordion] How long does it take for customers to receive their refunds?
>
> It depends on the customer's bank or payment method. Customers can typically expect to see refunds in their account within 3–7 business days after Whop processes them.


## Related topics

- [Managing Dispute Rates](https://docs.whop.com/trust-and-safety/account-health/managing-dispute-rates.md)
- [Local Payment Methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md)
- [Respond to Refund Requests in the Resolution Center](https://docs.whop.com/manage-your-business/manage-payments/resolution-center.md)
