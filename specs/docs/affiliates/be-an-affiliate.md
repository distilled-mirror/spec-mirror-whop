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

# Be an Affiliate

> Earn money by referring people to Whop offers using your affiliate link

You can earn money as an affiliate by referring people to join different Whop offers using your affiliate link.

## Become an affiliate

1. Under **Resources**, select **Affiliates**
2. Select **Refer buyers** to browse Whop's full Affiliate marketplace
3. You can browse the different accounts, commission rates, and earning stats for the different Whops running affiliate offers
4. Select the **Become affiliate** button for any offer to access your affiliate link
5. Select the **View assets** button and copy your affiliate link to get started

## How to withdraw your affiliate commissions

To withdraw, go to **Profile** > [Balance](https://whop.com/@me/settings/balance/) and select **Withdraw**.

There is usually a 14-day waiting period between when a referred customer makes their purchase and when the affiliate receives their commission. Some commissions can wait up to 30 days. If you don't see the payment in your balance, you may still be within the waiting period.

If a referred payment enters a dispute, receives a refund, or triggers fraud detection, Whop returns your affiliate commission to the business.

## Track your affiliate earnings

To track your performance and earnings as an affiliate:

1. Select **Affiliates** on the left-hand side of Whop
2. In the affiliates dashboard, you'll see your **Stats** section, which shows how much revenue you've earned from referrals


## Related topics

- [Affiliate Program](https://docs.whop.com/manage-your-business/growth-marketing/affiliate-program.md)
- [Set Up Global Affiliates](https://docs.whop.com/affiliates/setup-global.md)
- [Set Up Custom Affiliates](https://docs.whop.com/affiliates/setup-custom.md)
