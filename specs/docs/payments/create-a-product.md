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

# Set Up a Product to Accept Payments

> Add a product with free, one-time, or recurring pricing, set its currency and payment methods, and configure stock, waitlists, and checkout questions.

Products are how you package and price your offer on Whop. You can set up free access, one-time payments, or recurring subscriptions for people to access your whop.

## How to create a product

### 1. Add a product

* Go to your **Dashboard**
* Go to **Products**
* Select **Add Product**

### 2. Set up your product details

Configure the core details of your product:

* **Choose a title**: Keep it clear and descriptive (e.g., “Premium Membership,” “Full Course Access”)
* Select your pricing option:
  * **Free**: Give customers free access to your product
  * **One-time**: Charge customers once for lifetime access
  * **Recurring**: Set up monthly or yearly subscriptions
* **Choose currency**: Select the currency you want to charge in
* **Choose a price**: Enter how much customers will pay
  * If you chose recurring, also choose your billing cycle length (weekly, monthly, yearly)
* **Configure advanced options** (optional):
  * **Stock**: Limit how many people can purchase this product
  * **Add a waitlist**: Users enter payment details but aren’t charged until you approve them
  * **Ask questions before checkout**: Collect information from customers before they purchase
  * **Auto-expire access**: Automatically remove access after a set time
  * **Redirect after checkout**: Send customers to a specific page after they complete checkout
  * **Add internal name**: A name only you can see on your dashboard
  * **Visibility**: Make this product visible on your public whop store page
* **Choose payment methods**: Select what customers can pay with
  * Credit/debit card
  * Bank transfer (ACH)
  * PayPal
  * Cryptocurrency
  * Buy now, pay later (Financing)

> [!TIP]
>
> If you want to offer multiple pricing options (like both monthly and yearly plans), select **Add another billing period**.

When you’re happy with everything, select **Add Product**.

## Next steps

To start accepting payments on Whop, complete the final setup step:

> [card] [Copy checkout link](https://docs.whop.com/payments/create-checkout-link.md)
>
> Checkout links let you send customers straight to a payment page.


## Related topics

- [Create a Checkout Link to Accept Payments](https://docs.whop.com/manage-your-business/payment-processing/create-checkout-link.md)
- [Set Up Pricing](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
- [Set Up Custom Affiliates](https://docs.whop.com/affiliates/setup-custom.md)
