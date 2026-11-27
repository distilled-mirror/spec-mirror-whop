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

# Create a Waitlist

> Add a waitlist checkout option so people apply before joining your whop, ask them questions, and approve or deny each entry from your dashboard.

Use a waitlist to control who joins your whop. People apply to join, you review them, then approve the ones you want.

> [!TIP]
>
> You can view and manage all your waitlist entries by going to your Dashboard > **Waitlists**.

## How to create a waitlist checkout option

**To set up a waitlist for your whop:**

1. Go to your **Dashboard** > **Checkout links**
2. Select **+ Create checkout link**
3. Toggle **Advanced options** and select **Add a waitlist**
4. Select **Create**

Whop adds anyone who joins through your checkout link to the waitlist instead of giving them immediate access.

![Waitlist](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/how-to-videos/waitlist720p30fps1.gif?s=703ecafd3f73f08ee7425156f865af76)

## How to add questions to your waitlist

Pro tip: Add required questions for customers to answer before joining your waitlist. This will help qualify potential members and assess if they’re a good fit for your whop.

To add questions to your waitlist:

* Go to **Dashboard** > **Checkout links**
* Find your waitlist link and select the 3 dots (⋮)
* Select **Edit**
* Under **Advanced options**, select **Ask questions before checkout**
* Add your question titles and placeholder text
* Select **Update checkout link**

## How to approve/deny waitlist entries

Go to **Dashboard > Waitlists** to manage your waitlist. From here, you can:

* **Approve** (thumbs up button) or **deny** (thumbs down button) individual users
* **Admit all entries** at once by selecting the **+ Admit all entries** button
* **Export entries** by selecting the **Export** button in the top right
* **Contact users** on your waitlist via DM or email

When you approve someone, they'll automatically get access to your whop and receive a notification.

## When to use a waitlist

A waitlist works well when you want to:

* **Build anticipation while you're still creating**: Collect emails, build an audience, and validate demand without opening access right away.
* **Qualify potential customers**: Add custom questions to qualify leads before giving them access.
* **Create exclusivity for your community**: When people have to apply and wait for approval, it makes your whop feel more valuable and desirable.
* **Launch a limited early release** — Control who gets early access by reviewing applicants before letting them in. This is useful for testing with a smaller, high-quality group.


## Related topics

- [Create a Product and Choose a Pricing Strategy](https://docs.whop.com/manage-your-business/products/create-product.md)
- [Set Up Pricing](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
- [Manage Products](https://docs.whop.com/manage-your-business/products/manage-products.md)
