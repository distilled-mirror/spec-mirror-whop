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

# Access a Discord Server

> Link your Discord account to Whop and join Discord servers included with your membership.

To join a Discord server through Whop, you must first attach an existing Discord account to your Whop account. If you have already connected Discord to Whop, jump down to [Server access](#server-access).

## Linking your existing Discord account to Whop

Connect your Discord account to Whop to access the Discord servers included with your Whop membership.

### 1. Sign in to your account on Whop

Sign in at [whop.com](https://whop.com).

### 2. Go to External accounts

Go to the [**External
accounts**](https://whop.com/@me/settings/external-accounts/) settings page
on Whop to view and edit your account settings.

### 3. Connect Discord

Select the **➕** icon or **Connect** next to Discord.

### 4. Complete the redirect to Discord

Whop opens Discord in a new browser tab. On the Discord site,
log into your Discord account.

### 5. Authorize Whop Bot

When a form titled **Whop Bot** appears, scroll down and select **Authorize**. Discord then redirects you back to Whop.

**On mobile:** Open your mobile browser, go to the same [External accounts](https://whop.com/@me/settings/external-accounts/) page, tap **Add Account**, and follow the same steps to log into Discord and authorize Whop.

## Server access

If your Whop membership includes access to a Discord server, go to the whop's page after linking your account, then select the server and claim access. Read the guide on [How to find your purchase](https://docs.whop.com/memberships-and-access/accessing-your-purchase/how-to-find-your-purchase.md) if you need help finding the whop you bought.

### 1. Open the Whop page with the Discord server

Open the Whop page that includes the Discord server you want to access.

### 2. Select the Discord app in the whop

In the whop, select the **Discord** app.

### 3. Select your Discord account and claim access

Choose your linked Discord account and select **Claim Access**.

### 4. You're done

Whop opens Discord, where you should now have access to the Discord community.


## Related topics

- [Transfer a Membership](https://docs.whop.com/memberships-and-access/accessing-your-purchase/transfer-a-membership.md)
- [Access Content on Mobile](https://docs.whop.com/memberships-and-access/access-discord-server/access-content-on-mobile.md)
- [SaaS](https://docs.whop.com/supported-business-models/saas.md)
