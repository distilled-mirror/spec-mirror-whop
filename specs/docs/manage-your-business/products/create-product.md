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

# Create a Product and Choose a Pricing Strategy

> Create a product with its name, pricing, apps, and checkout options, then pick a free or paid product strategy, with best practices and troubleshooting tips.

Create a product to define what you sell, how much it costs, and which apps or content members will access after purchase.

## Create a product

### 1. Go to your Dashboard

1. Go to your **Dashboard**
2. Select **[Products](https://whop.com/dashboard/products/)** then **Create product**

### 2. Explain what you're selling

1. Enter a **name** for your product (maximum of 80 characters) 2. Enter a
   **headline** to describe what you're selling (maximum of 80 characters) 3. If you
   need creative inspiration, select the star icons to **generate using AI** 4.
   Add a **banner image** that represents your product

### 3. Choose pricing

1. Choose your **pricing** (one-time, subscription, or free) and enter the price
   — you can create multiple pricing options for your product 2. Choose your
   **currency**, add **waitlists**, include **free trials**, and enable
   **advanced payment options** 3. Customize which **payment methods** you want
   customers to see on the checkout page

### 4. Add apps

1. Select the **Next** button in the top right 2. Add **apps** or experiences
   members will get access to with their purchase 3. If you're using Whop only to
   collect payments and are fulfilling off-platform, you don't need to add apps

### 5. Design your product page

1. Add a **photo or video** that represents your product
2. Write a **description** to explain what you're selling

> [!TIP]
>
> If you want to unlock features such as financing or higher checkout links, Whop needs to understand what you're selling. A detailed product page helps Whop approve your business for these features.

### 6. Save your product

1. Select **Create** to save
2. You can change pricing, apps, and settings any time from the product page

## Free product strategy

**Reasons to create free products**

* Capture potential customer emails
* Build initial audience
* Provide value upfront
* Create a sales funnel entry point

**Free product creation process**

1. Navigate to the **Products** tab
2. Select **Create product**
3. Select **Free product**

**Configuration**

* **Title:** Be specific and clear (e.g., "Basic AI Agency Course")
* **Headline:** Use AI generation or craft manually
* **Description:** Explain the product value proposition
* **Media:** Add relevant images or videos

**Advanced options**

* Set maximum participants (e.g., 100 members)
* Create a waitlist
* Add pre-checkout questions
* Configure post-checkout redirects

### Paid product strategy

**Reasons to create paid products**

* Generate direct revenue
* Provide advanced or premium content
* Segment your audience
* Offer deeper value

**Paid product creation process**

1. Navigate to the **Products** tab
2. Select **Create product**
3. Select **Paid product**

**Configuration**

* **Title:** Reflect your premium offering (e.g., "AI Mentorship")
* **Pricing:** Set appropriate value (e.g., \$1,000)
* **Payment methods:** Offer multiple options
* **Features:** Clearly list benefits
* **FAQs:** Anticipate and answer potential customer questions

## Advanced strategies

* Create complementary free and paid products
* Use free products as lead magnets
* Develop a clear progression path for customers
* Continuously update and improve product offerings

## Best practices

* Keep product descriptions clear and concise
* Use high-quality media
* Update your offerings
* Respond to customer feedback
* Monitor product performance

## Troubleshooting

* Complete all required fields
* Double-check pricing and settings
* Test your product page before publishing
* Verify link accessibility


## Related topics

- [Set Up Pricing](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
- [Manage Products](https://docs.whop.com/manage-your-business/products/manage-products.md)
- [Create Checkout Links for Your Products](https://docs.whop.com/payments/create-checkout-link.md)
