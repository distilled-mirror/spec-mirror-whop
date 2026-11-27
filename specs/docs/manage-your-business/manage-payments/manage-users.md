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

# Manage Users

> See your users and their memberships in the Whop dashboard, and edit, pause, or cancel a membership.

On the dashboard for your business, select the [Users](https://whop.com/dashboard/users/) tab. From this page, you can view all your users' details and manage their memberships.

## Users table

From the Users table, you will be able to see the following information about each of your users:

* Name
* Email address they used to purchase the membership
* Status of their membership (joined, left, paused, churned, renewing, or paid once)
* Their country, state, and city
* Their total spend
* When they joined and last accessed your product
* Whop disputes, reviews, resolutions, and total spend on Whop

You can filter and export any of this user data using the buttons at the top of the table.

> [!TIP]
>
> If you need help, Whop AI can help create reports around any user and
> membership data you might need.

You can also contact any user via Whop DM, their email address, or any other linked socials from this page.

## Memberships table

Select the [Memberships](https://whop.com/dashboard/users/?tab=memberships) tab to see all your users' individual memberships. A user appears only once on the Users table, but each membership appears on this page.

The memberships table lets you see:

* User name
* Email
* Product name and plan (plan is the specific priced plan for that membership)
* Status of that membership — is it currently on a trial, renewing, one-time, canceling, etc.
* Total spend on that membership
* Membership creation and cancellation date

You can also filter and export all the information from this page.

## Editing a membership

If you need to edit a customer's membership, you can do so from this page. Select the **three-dot menu** next to the membership to make any of the following changes:

* **Generate transfer link** — create a link that transfers this membership to another user
* **Ban user** — permanently ban the user from your whop
* **Add free days** — extend the customer's access by pushing back their next renewal date
* **Apply a promo code** — apply a discount code to the membership
* **Pause payments** — temporarily pause billing for the membership
* **Cancel membership** — the user retains product access until the next billing date, when Whop stops billing and removes access
* **Terminate membership** — immediately revoke the user's access to their membership, with no further access to the product (you can terminate both one-time and subscription memberships)
* **Access details** — view the membership, member, plan, and product IDs

### How to add free days to a plan

To extend a customer's access or push back their next renewal date, you can add free days to their membership.

1. Go to **Dashboard** > **Users** and select the **Memberships** tab.
2. Find the membership you want to adjust.
3. Select the **three-dot menu** next to the membership and select **Add free days**.
4. Enter the number of free days you want to add and confirm.

Adding free days moves the customer's next charge date by the number of days you choose.


## Related topics

- [Manage Disputes](https://docs.whop.com/manage-your-business/manage-payments/manage-disputes.md)
- [Business Analytics Dashboard](https://docs.whop.com/manage-your-business/manage-business/analytics.md)
- [Manage Products](https://docs.whop.com/manage-your-business/products/manage-products.md)
