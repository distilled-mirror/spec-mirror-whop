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

# Business Analytics Dashboard

> See how your business is growing on the Whop dashboard, and add metric widgets for the numbers you check most.

Whop’s built-in analytics help you understand how your business is growing, your financial performance, and who your customers are.

## How to view your analytics

To view your analytics:

1. Go to your **Dashboard**
2. Select **Home**

### How to add analytics widgets to your Dashboard

Widgets are metric cards that show your most important business stats at a glance. Choose which metrics you want to track and customize your dashboard layout.

Here's how:

1. Go to **Dashboard** > **Home**
2. Select **+ Add** on the right side under **Stats**
3. Select **+** on the widgets you want
4. Select **Apply** when you're done
5. Drag them around to organize them how you want
6. Select **Done** (next to **+ Add**) to save your new widget layout

> [!TIP]
>
> The dashboard organizes widgets into three main categories: Users,
> Payments, and Resolution Center. Each category provides specific stats to help
> you understand different aspects of your business performance.

![Analytics](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/how-to-videos/analytics720p30fps.gif?s=538715e28fb115ca2cbaac9c7f8d2899)

## Understanding your analytics data

### 1. User analytics

The Users category helps you understand who your customers are and how they interact with your business.

**Customer engagement metrics**

* **Top users**: The members that have spent the most amount (excluding refunds and disputes)
* **New users**: Total number of new users in the specified time. This doesn't include existing users who renew their memberships or add additional subscriptions
* **Time spent** : The amount of time spent by users in your whop
* **Page visits**: Estimated number of visits to your store pages in the specified period of time
* **Users breakdown**: Total number of users per status (e.g. Joined, Renewing, Trialing, Paused)

**User status breakdown**

* **Paid active users**: Total number of paid active users in your whop in the specified time
* **Users with app installed**: The number of users with an active membership who have the mobile app installed

**Conversion & Revenue metrics**

* **Spend per paying user**: Estimated revenue for all payments made by customers, divided by the total number of customers who have made a payment
* **Trial conversion rate**: The percentage of users that converted from a trial to an active subscription in the specified period of time
* **Top affiliates**: The affiliates who have generated the most revenue for your whop

### 2. Payments analytics

The Payments category tracks your financial performance with detailed payment and revenue metrics.

**Core revenue metrics:**

* **Gross revenue**: Revenue from payments made to your Whop account
* **MRR (monthly recurring revenue)**: Estimated monthly revenue from all active and past-due subscriptions
* **ARR (annual recurring revenue)**: Estimated annual revenue from all active and past-due subscriptions
* **Revenue (after Whop fees)**: Estimated revenue after Whop fees in the specified time

**Payment transaction data:**

* **Payments breakdown**: All payment volume in the specified time per status (Paid, Failed, Past due, Canceled, Refunded, Pending)
* **Successful payments**: Total number of successful payments in the specified time
* **Payment processing fees**: Total transaction fees charged by payment processors
* **Spend per subscription**: Estimated revenue for subscription payments, divided by the total number of subscribers

**Business health indicators:**

* **Churn rate**: Churned users in the past 30 days divided by active users 30 days ago plus new users in that period
* **Churned revenue**: The total monthly recurring revenue lost in the specified time due to churn

**Tax and Fee management:**

* **Sales Tax Withheld**: Estimated sales tax withheld in the specified time
* **Affiliate Fees**: Estimated total fees paid to affiliate referrals in the specified time

**Disputes:**

* **Disputes Count**: Total number of disputes received in the specified time
* **Dispute Alerts Count**: Total number of alerts received in the specified time

> [!TIP]
>
> Payment stats help you spot trends, catch problems early, and make smart
> pricing decisions.

### 3. Refund request analytics

The Resolution Center category shows you:

* **Total Refunded**: Total amount refunded to customers in the specified time


## Related topics

- [Tracking Integrations](https://docs.whop.com/manage-your-business/growth-marketing/tracking-integrations.md)
- [Whop Ads](https://docs.whop.com/manage-your-business/growth-marketing/ads.md)
- [Create a Checkout Link to Accept Payments](https://docs.whop.com/manage-your-business/payment-processing/create-checkout-link.md)
