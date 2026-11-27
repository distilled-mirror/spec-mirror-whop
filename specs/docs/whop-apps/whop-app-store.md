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

# The Whop App Store

> Browse the Whop App Store from your whop, filter apps by category or by newest, trending, and most addicting, and add the ones that fit your community.

The Whop App Store is a marketplace where you can browse and install apps for your whop.

## Browse apps

To start browsing available apps:

* Log in and go to your whop
* Select **Add app** from the left sidebar
* You're now in the Whop App Store and ready to explore

## Find apps

1. **Browse by category.** Select a category tab: **Community**, **Business & Productivity**, **Games**, **Sports**, **Social Media**, **Trading**, **AI**, **Education**, **E-commerce & Shopping**, **Health & Fitness**, or **Travel**.
2. **Filter your results** to discover the best apps:
   * **Newest**: See the latest apps that just dropped
   * **Trending**: Find what's getting the most installs right now
   * **Most Addicting**: Discover which apps people are spending the most time using

When you find an app that fits your community, select **Add**.

> [!TIP]
>
> The Whop App Store's selection continues to grow. Check it often to discover
> new apps your members will find useful.

![Whop App Store](https://mintcdn.com/whop/XkQkLHsY-Hk8KVyG/how-to-videos/whopappsfiltersandcategories720p30fps.gif?s=2017d85eddb9cbeb463d4f12d4810238)


## Related topics

- [Add Apps](https://docs.whop.com/add-apps.md)
- [Content Rewards](https://docs.whop.com/memberships-and-access/third-party-apps/content-rewards.md)
- [Agencies](https://docs.whop.com/supported-business-models/agency-services.md)
