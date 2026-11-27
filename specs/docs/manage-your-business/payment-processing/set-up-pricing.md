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

# Set Up Pricing

> Create a product to offer free, one-time, or recurring pricing for your whop, and decide how many tiers to offer.

Products are how you package and price your offer on Whop. You can set up free access, one-time payments, or recurring subscriptions for people to access your whop.

## How to set up pricing for your whop

### 1. Add a product

To set up pricing, you need to create a product:

* Go to your whop
* Select **Products**
* Select **Add product**

### 2. Set up your product details

Configure the core details of your product:

* **Choose a title**: Keep it clear and descriptive, such as Premium Membership or Full Course Access
* Select your pricing option:
  * **Free**: Give customers free access to your whop
  * **One-time**: Charge customers once for lifetime access
  * **Recurring**: Set up monthly or yearly subscriptions
* **Choose currency**: Select the currency you want to charge in
* **Choose your price**: Enter how much customers will pay
  * If you chose recurring, also choose your billing cycle length (weekly, monthly, yearly)
* **Configure advanced options** (optional):
  * **Stock**: Limit how many people can purchase this product
  * **Show on store page**: Make this product visible on your public store page
  * **Add a waitlist**: Users enter payment details but aren't charged until you approve them. See [Create a waitlist](https://docs.whop.com/manage-your-business/products/create-waitlist.md)
  * **Ask questions before checkout**: Collect information from customers before they purchase
  * **Auto-expire access**: Automatically remove access after a set time
  * **Redirect after checkout**: Send customers to a specific page after they complete checkout
  * **Add internal name**: A name only you can see on your dashboard
* **Choose payment methods**: Select what customers can pay with
  * Credit/debit card
  * Local payment methods and bank transfers
  * Cryptocurrency and wallets
  * Buy now, pay later (Financing)

> [!TIP]
>
> If you want to offer multiple pricing options (like both monthly and yearly plans), select Add another billing period.

### 3. Choose which apps customers get with this product

This is where you decide what customers actually get when they purchase your product. When users purchase or claim your product, they will get access to the apps you select.

* **Toggle on** all the apps you want to include in this product
* **Toggle off** any apps you don't want included in this product

> [!TIP]
>
> You can create multiple products with different app combinations. For example, create a free product that gives access to basic apps, and a premium product that unlocks premium apps.

When you're happy with everything, select **Add Product**.

## Understanding products: Different ways to structure your whop

Your whop can contain multiple products, each acting like a different membership option. You might have a basic tier and premium tier, or just one all-access option.

**Common examples:**

* **One product:** "Full Access" (\$49/month) with all apps included
* **Two products:** "Free Community" (chat only) + "Premium" (\$29/month, all apps)
* **Three products:** Basic (\$19), Pro (\$49), and Premium (\$99) with different app combinations

> [!TIP]
>
> Start simple with one product and add more later as your business grows.


## Related topics

- [Add Apps](https://docs.whop.com/add-apps.md)
- [Launch Your Business](https://docs.whop.com/launch-your-business.md)
- [Educational Programs](https://docs.whop.com/supported-business-models/educational-programs.md)
