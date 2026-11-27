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

# Hide Approximate Location

> Hide or show your approximate location on your Whop profile, on the web or in the Whop app.

You can control whether your approximate location is visible to other users on Whop. Turn the setting off to hide it. While you hide your location, you also can't see the approximate location of other users. To change the name others see, [update your username](https://docs.whop.com/account-settings/personal-details/update-username.md).

## How to hide your approximate location on the web

1. Sign in to your account on Whop.
2. Go to [Account settings](https://whop.com/@me/settings/profile/).
3. Select the **Profile** tab.
4. Scroll to the **More details** section at the bottom of the page.
5. Turn off the **Location** switch.

## How to hide your approximate location in the Whop app

1. Open the Whop app on iOS or Android and go to your profile.
2. Tap **Edit profile**.
3. Turn off **Show approximate location**.
4. Tap the check mark at the top of the screen to save.


## Related topics

- [Update Username](https://docs.whop.com/account-settings/personal-details/update-username.md)
- [Local Payment Methods](https://docs.whop.com/payments-and-billing/local-payment-methods.md)
- [Apply for Financing](https://docs.whop.com/payments-and-billing/financing/apply.md)
