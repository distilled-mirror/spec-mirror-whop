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

# Manage Products

> Update a product's pricing and billing options, choose and organize the apps members get, preview what members see, and create new products.

Update your product pricing, change which apps your customers get access to, and create new products or billing options.

## Update an existing product

### 1. Select the product you want to change

1. Go to your Dashboard
2. Select **Products**
3. Select the product you want to update

### 2. Update the title and description

1. Update the **title** and **description** directly in the main fields 2.
   Select **Save Changes** when done

### 3. Change pricing

1. Select the current **price** – a dropdown menu will appear
2. Enter the new amount your product should cost
3. Select **Save Changes**

> [!TIP]
>
> You can only update the price amount here, not the pricing model (i.e. free, one-time payment, or recurring). To offer a different pricing model, you need to add a new billing period (see 'Add a new pricing model' below).

### 4. Add a new pricing model

You can have multiple pricing models for the same product (e.g. a monthly subscription AND a yearly subscription AND a one-time fee for lifetime access).

To add a new pricing model:

1. Select **Add another visible billing period**
2. Select the pricing type (free, one-time, or recurring)
3. Choose the currency and enter the price
4. Configure billing cycle (for recurring plans)
5. Select **Save Changes**

### 5. Update advanced settings

Select **Advanced options** to modify:

* **Stock**: Limit how many people can purchase this product
* **Show on store page**: Make this product visible on your public store page
* **Add a waitlist**: Users enter payment details but aren't charged until you approve them. See [Create a waitlist](https://docs.whop.com/manage-your-business/products/create-waitlist.md)
* **Ask questions before checkout**: Collect information from customers before they purchase
* **Auto-expire access**: Automatically remove access after a set time
* **Redirect after checkout**: Send customers to a specific page after they complete checkout
* **Add internal name**: A name only you can see on your dashboard

### 6. Update payment methods

1. Select **Payment methods**
2. Select or deselect the options you want to offer:
   * Credit/debit card
   * Local payment methods and bank transfers
   * Cryptocurrency and wallets
   * Buy now, pay later (Financing)
3. Select **Save Changes**

## Choose apps for a product

Control what customers get access to when they purchase each product by selecting (and deselecting) which apps to include.

**To update app access:**

1. Go to **Dashboard** > **Products**
2. Select the product you want to change
3. In the right-hand column, you'll see all your available apps
4. **Toggle on** apps you want included in this product
5. **Toggle off** apps you want to exclude
6. Changes save automatically

> [!NOTE]
>
> **Example scenario:**
>
> * **Free product**: Includes only a free chat app and a beginners course (excluding all premium apps)
> * **Premium product**: Also includes premium apps (e.g. advanced learning materials and templates)
> * **VIP product**: Includes an extra app that gives VIP members the ability to book 1-on-1 calls with you

## Create a new product

You can create multiple membership tiers (e.g. free and premium) by creating multiple products.

To create a new product:

1. Go to your whop
2. Select **Products**
3. Select **Create product**

> [card] [Set up your product pricing and details](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
>
> Learn more about how to configure the settings of your product here.

## Preview what members see in each product

Members will only see apps they have access to based on the products they have joined and purchased.

**To see what your product looks like for your members:**

1. Go to your whop's main page
2. Select the **Preview as** dropdown menu
3. Select the product you want to preview

## Organize your apps and categories in each product

You can create categories to group your apps and drag both categories and apps to keep your whop looking clean and organized.

**To create categories:**

1. Right-click in the left-hand app column (where you can see the list of all your apps)
2. Select **Create category**
3. Name your category
4. Select **Create**

**To rename categories:**

1. Right-click on the category
2. Select **Edit category**
3. Update the name
4. Select **Save**

**To rename apps:**

1. Right-click on the app you want to update
2. Select **App settings**
3. Update the name
4. Select **Save**

> [!TIP]
>
> There are 2 ways to organize your apps when managing multiple products:
>
> 1. Organize by product tier: Create categories like "Free Members" and "Premium Members" and group relevant apps under each category
> 2. Organize by content theme: Create categories such as Community, Learning, and Events. Use names such as Free Chat or Premium Chat to show each app's access level.


## Related topics

- [Team Management](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)
- [Delete Your Account](https://docs.whop.com/account-settings/account-security/delete-your-account.md)
- [Manage Users](https://docs.whop.com/manage-your-business/manage-payments/manage-users.md)
