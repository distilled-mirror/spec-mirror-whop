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

# Promo Codes

> Create promo codes that take a percentage or fixed amount off at checkout, and expire them early when you need to.

Use promo codes to offer percentage discounts (like 10% off) or fixed amounts (like \$10 off) that people can apply at checkout to join your whop.

[YouTube video player](https://www.youtube.com/embed/tIdnAPTwt7g?si=jtsX6HJOG-Lh3KlE\&rel=0)

## How to create a promo code

### 1. Access your dashboard

Head over to your **Dashboard.**

> [!TIP]
>
> Make sure you're viewing the whop you want to create a discount for (using the whop selection modal at the top left).

### 2. Navigate to Promo codes

Select the **Marketing** dropdown and select **Promo codes**. You'll see any existing promo codes you've already created for your whop here.

### 3. Create a new code

If this is your first promo code, select **+ Create Promo Code** in the center of your page. If you've already created a promo code (and you want to create a new one) select **+ Create promo code** at the top right of your screen. A popup will appear where you'll set up your code.

### 4. Set up your promo code

Configure the settings for your promo code:

1. **Code**: This is the actual promo code name, like "SAVE20" or "WELCOME10." Enter a name for your code.
2. **Discount**: Decide how much your discount will be
   * Use the dropdown to pick either a **Percentage** (like 10% off) or a **Fixed amount** (like \$10 off).
   * Then, enter the discount value in the value field.
3. **Discount duration:** Decide how long the discount applies
   * **One-time (default)**: Customers receive the discount only once.
   * **Forever**: The discount applies to all future payments.
   * **Multiple months**: The discount applies for a specific number of months (a field will pop up for you to enter the number).
4. **Eligible users**: Choose who can use this code.
   * **Everyone (default)**: Anyone can use it.
   * **Only new**: Only new customers can use this code.
   * **Only churned**: Only previous customers who have left can use it.
5. **Expires**: Choose when the promo code expires using the date and time selector. By default, the code expires one month after creation.
6. **Usage limit section**: Enter the maximum number of times customers can use this code. The default limit is 100 uses.
7. **One per customer**: By default, each customer can use the promo code only once. Turn this off to let customers reuse the code.
8. **Apply to specific whops**: This is off by default. If you turn it on, you can limit the promo code to certain checkout links within your organization.

### 5. Activate your code

Select **Create** at the bottom of the popup to make your promo code live.

## Manually expire a promo code

If you need to deactivate a promo code before its set expiration date, you can manually expire it.

1. **Access the promo code options**: Select the three dots (︙) next to the promo code you want to expire.
2. **Select Expire**: Select **Expire** from the dropdown menu that appears.
3. **Confirm expiration**: A confirmation message will pop up. Select **Expire** to permanently deactivate the promo code.

> [!WARNING]
>
> After a promo code expires, you can't reactivate it. If you want to offer
> that same discount again, you'll need to create a brand new promo code.


## Related topics

- [Team Management](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)
- [Whop Ads](https://docs.whop.com/manage-your-business/growth-marketing/ads.md)
- [Manage Users](https://docs.whop.com/manage-your-business/manage-payments/manage-users.md)
