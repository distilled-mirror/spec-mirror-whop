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

# Create Checkout Links for Your Products

> Turn a product into a shareable payment page with a free, one-time, recurring, or split payment checkout link, and edit or embed the link later.

Checkout links let you send customers straight to a payment page to buy your products or services. You can offer free access, one-time payments, or monthly subscriptions.

Checkout links are the fastest way to get paid for your products and services. Here's how different businesses use them:

* **Sales teams** — Create custom links for each deal
* **Event hosts** — Send direct links to webinar or workshop checkout pages
* **Freelancers** — Use checkout links as digital invoices for completed work

[YouTube video player](https://www.youtube.com/embed/L4f9rPtlCuc?si=n1OLK7Y42fkVKgse\&rel=0)

## Find and create checkout links

* Go to your **Dashboard** > **Checkout links** to see all your links
* Select **+ Create checkout link** to create a new one

> [!TIP]
>
> Find every pricing option you create on your whop at Dashboard > Checkout links.
>
> Find every checkout link you create on your dashboard at whop > Manage pricing.

## How to create a checkout link

You can create three types of checkout links:

* **Free links** - Give customers free access to your whop
* **One-time payment links** - Charge customers once
* **Recurring payment links** - Set up monthly or yearly subscriptions

> [!TIP]
>
> Each checkout link connects to one of your whops. When you create the link
> from your whop's pricing page, it's automatically connected to that whop. When
> you create it from your Dashboard, you'll need to choose which whop it's for.

### Free checkout links

**To create a free checkout link:**

1. Choose **Free** under **Pricing type**
2. Select **Advanced Options** to add these features:
   * **Stock** — Limit how many people can use this link (or keep unlimited)
   * **Description** — Explain what users get when they join
   * **Show on store page** — Make this option visible on your public store page
   * **Add a waitlist** — Users enter card details but aren't charged until you approve them
   * **Ask questions before checkout** — Collect info from users before they join. You can make these optional by checking the **Optional** box.
   * **Auto-expire access** — Automatically remove access after a set time
   * **Redirect after checkout** — Send users to a specific page after they complete checkout
   * **Add internal name** — A name only you can see on your dashboard

### One-time payment links

**To create a one-time payment link:**

1. Choose **One-time** under **Pricing type**
2. Enter your **Price**
3. Select **Advanced Options** to add these features:
   * **Stock** — Limit how many people can use this link (or keep unlimited)
   * **Description** — Explain what users get when they join
   * **Show on store page** — Make this option visible on your public store page
   * **Add a waitlist** — Users enter card details but aren't charged until you approve them
   * **Ask questions before checkout** — Collect info from users before they join. You can make these optional by checking the **Optional** box.
   * **Auto-expire access** — Automatically remove access after a set time
   * **Redirect after checkout** — Send users to a specific page after they complete checkout
   * **Add internal name** — A name only you can see on your dashboard
4. Choose your **Payment methods** (what customers can pay with):
   * Credit/debit card
   * Bank transfer (<abbr title="Automated Clearing House">ACH</abbr>)
   * PayPal
   * Cryptocurrency
   * Buy now, pay later (Financing)

### Subscription (recurring) links

To create a subscription link:

1. Choose **Recurring** under **Pricing type**
2. Enter your subscription details:
   * **Subscription price** — How much customers pay each billing cycle
   * **Subscription period** — How often they're charged (weekly, monthly, yearly)
   * **Initial fee** — Optional one-time setup fee when they first join
3. Select **Advanced Options** to add these features:
   * **Stock** — Limit how many people can use this link (or keep unlimited)
   * **Description** — Explain what users get when they join
   * **Show on store page** — Make this option visible on your public store page
   * **Add a waitlist** — Users enter card details but aren't charged until you approve them
   * **Ask questions before checkout** — Collect info from users before they join. You can make these optional by checking the **Optional** box.
   * **Auto-expire access** — Automatically remove access after a set time
   * **Redirect after checkout** — Send users to a specific page after they complete checkout
   * **Add internal name** — A name only you can see on your dashboard
   * **Free trial** — Let customers try before they pay (choose how many days)
   * **Split payments** — Let customers pay in installments instead of all at once
   * **Cancellation discount** — Offer a discount when someone tries to cancel (helps retain customers)
4. Choose your **Payment methods**:
   * Credit/debit card
   * PayPal

## Create a split payment checkout link

A split payment checkout link lets you charge customers in multiple installments for a product instead of collecting the full amount upfront.
Whop automatically charges each payment on a recurring schedule until the customer completes all installments.
After the customer pays the final installment, Whop stops charging them automatically.

### 1. Go to Checkout links

From your Whop dashboard, select **Checkout links** in the left-hand menu.

### 2. Create a new link

Select **+ Create checkout link** in the top right.

### 3. Choose product & set pricing

Select the **Product** and add a **Description** (optional).
Under **Pricing**, choose **Recurring**, enter the **price**, and select the **billing interval** (e.g., **/ 1 month**).

### 4. Open Advanced options

Select **Advanced options** to reveal more settings.

### 5. Enable split payments (installments)

Check **Enable split pay (Installments)**.
This converts the recurring plan into a fixed installment plan and will **automatically charge your customer** each period until they’ve completed all installments.

### 6. Set installments & finalize

Enter the **number of payments** (installments) and confirm the **price per billing period**.

**Example:**
If a customer wants to pay **\$1,000 over 4 months**, set:

* **Price:** **\$250**
* **Billing interval:** **/ 1 month**
* **Installments:** **4**

The customer will be automatically charged **\$250 per month** for **4 months**, then their payments will stop once all installments are complete.
(You can adjust the billing duration — e.g., weekly — if you prefer a different schedule.)

### 7. Create the checkout link

When everything looks good, select **Create checkout link** at the bottom to generate your split payment link.

> [!TIP]
>
> Split payments aren't the same as financing. With split payments, you receive
> each installment as the customer pays it. With financing, a financing provider
> pays you the full amount upfront, then collects payments from the customer.

## How to edit checkout links

If you need to update or change your pricing and settings, you can edit your checkout links from either your Dashboard or your whop's pricing page.

### Open your Whop dashboard

* Go to **Dashboard** > **Checkout links**
* Find your link and select the three dots (︙) on the right
* Select **Edit**
* Make your changes and select **Update checkout link**

> [card] [Create tracking links](https://docs.whop.com/manage-your-business/growth-marketing/tracking-links.md)
>
> Tracking links show you how your links perform, including their clicks,
> conversion rates, and generated revenue.

## Embed Whop Payments on your website

> [card] [Checkout element](https://docs.whop.com/elements/latest/checkout/overview)
>
> Mount a complete checkout form right into your website with Whop Elements.

## Frequently asked questions

> [accordion] What exactly are checkout links?
>
> Checkout links (also called pricing options) are custom URLs that let
> customers join your whop with free access, a one-time payment, or a
> recurring subscription.

> [accordion] How do I limit how many people can use a link?
>
> In the Advanced Options of any pricing type, look for the "Stock" setting.
> It's set to unlimited by default, but you can change it to any number you
> want.

> [accordion] Can I change the currency for my prices?
>
> Yes. When setting the price for one-time or recurring links, you'll see a
> currency dropdown next to the price field.

> [accordion] How do I offer a free trial?
>
> Free trials are only available for recurring (subscription) pricing. You'll
> find the free trial option in the Advanced Options section when creating a
> recurring link.

> [accordion] How do I create a private link that doesn't show on my store page?
>
> When creating any checkout link, leave the "Show on store page" option
> unchecked. The link will still work when you share it directly, but it won't
> appear publicly on your store.

> [accordion] Why aren't my checkout links showing up on my store page?
>
> Check that you enabled "Show on store page" in the Advanced Options when you
> created the link. If this option is off, the link will only work when shared
> directly.


## Related topics

- [Create a Checkout Link to Accept Payments](https://docs.whop.com/manage-your-business/payment-processing/create-checkout-link.md)
- [Create a Waitlist](https://docs.whop.com/manage-your-business/products/create-waitlist.md)
- [Create a Whop](https://docs.whop.com/payments/create-a-whop.md)
