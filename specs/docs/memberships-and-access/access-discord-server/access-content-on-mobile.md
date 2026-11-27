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

# Access Content on Mobile

> Open your whops and content on your phone with the Whop app for iOS or Android, or in a mobile browser, using the same email as your memberships.

You can access your whops and content on your phone using the Whop app or a mobile browser.

## Whop mobile app

1. Install the **Whop** app from the App Store (iOS) or Google Play (Android)
2. Log in with the same email you use for your memberships
3. Open the app and tap **Your whops** to see your memberships
4. Tap a whop to open it and use chat, courses, Discord linking, and other content

## Mobile browser

1. Go to [whop.com](https://whop.com) in your phone’s browser
2. Log in with your Whop account
3. Use the menu to open **Your whops** and tap a whop to access it

Discord access, TradingView claims, and other linked services may require you to complete their own steps in their apps or on their sites after you’ve joined the whop.

More content coming soon.


## Related topics

- [How to Find Your Purchase](https://docs.whop.com/memberships-and-access/accessing-your-purchase/how-to-find-your-purchase.md)
- [Content Rewards](https://docs.whop.com/memberships-and-access/third-party-apps/content-rewards.md)
- [Access a Discord Server](https://docs.whop.com/memberships-and-access/access-discord-server/access-a-discord-server.md)
