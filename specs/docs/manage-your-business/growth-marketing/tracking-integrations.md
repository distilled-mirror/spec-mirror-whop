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

# Tracking Integrations

> Connect Google Analytics, Hyros, Meta, and TikTok to your whop by adding your platform IDs, so you can see where traffic comes from and how your ads perform.

Whop integrates with major analytics platforms so you can see the full picture of your marketing and monitor your whop's traffic. For revenue, customer, and refund metrics without connecting anything, use Whop's [built-in analytics](https://docs.whop.com/manage-your-business/manage-business/analytics.md).

## Supported platforms

* Google Analytics
* Hyros
* Meta
* TikTok
* X (formerly Twitter)
* Reddit
* Pinterest
* HubSpot

## What these integrations track

When someone visits your whop, Whop sends data to these platforms so you can:

* See traffic from all your marketing
* Know where visitors come from
* Track how well your ads perform
* Connect page views to specific campaigns

## How to add platform IDs

To connect any analytics platform:

1. Go to your **Dashboard** → **Settings**.
2. Under **Analytics**, select the platform you want to integrate with your whop.
3. Enter your tracking IDs and API keys.
4. Select the blue checkmark to activate the integration.

> [!TIP]
>
> There are tutorials under each platform to help you find Pixel IDs and API
> keys.


## Related topics

- [Tracking Links](https://docs.whop.com/manage-your-business/growth-marketing/tracking-links.md)
- [Whop Partner Program Guide](https://docs.whop.com/partners.md)
- [Whop Ads](https://docs.whop.com/manage-your-business/growth-marketing/ads.md)
