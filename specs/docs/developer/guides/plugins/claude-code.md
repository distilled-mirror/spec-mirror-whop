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

# Whop Plugin for Claude Code

> Install the Whop plugin for Claude Code to manage your Whop business from the terminal.

The Whop plugin connects Claude Code to your live Whop business, so you can create products, take payments, run ads, and read your numbers.

The plugin bundles the [MCP server for the Whop API](https://docs.whop.com/developer/guides/ai_and_mcp.md) with skills
that teach Claude how to use it.

## Setup

**1. Add the marketplace**

```
/plugin marketplace add whopio/plugins
```

**2. Install the plugin**

```
/plugin install whop@whop
```

**3. Restart Claude Code**

Claude Code loads a newly installed plugin on the next start.

**4. Authorize**

Run `/mcp`, select `whop`, and authenticate. Your browser opens to Whop to sign in
and approve access.

**5. Confirm the connection**

```
/whop:whop-connect
```

Claude reports who you are signed in as, which businesses the grant can reach, and
how many tools are available.

## Using it

Explain what you want to do. Claude picks the right tool and passes your business id:

```
List my products and this month's revenue
Create a $24 one-time product called "Dark Chocolate Sampler"
Which memberships churned last week?
Draft an ad campaign for my store, but keep it as a draft
```

If you manage more than one business, say which one.

## Anything that moves money asks first

Financial, credential, and destructive operations run in two steps. The first call
**prepares** the operation and returns a preview. Nothing happens until you approve it.

Ask Claude to issue a refund and you get the operation, the target, and whether it's
reversible — then it stops and waits.

> [!WARNING]
>
> The plugin requests the `admin` permission profile, which covers every business your
> Whop user manages. Disconnect it from `/mcp` if you no longer need it.

## Next steps

> [card] [Grok](https://docs.whop.com/developer/guides/plugins/grok.md)
>
> The same plugin for Grok Build.

> [card] [AI and MCP](https://docs.whop.com/developer/guides/ai_and_mcp.md)
>
> Connect any MCP client, or the docs server.

> [card] [API overview](https://docs.whop.com/api-reference/overview.md)
>
> How requests, versioning, and pagination work.

> [card] [Test in sandbox](https://docs.whop.com/developer/guides/sandbox.md)
>
> Test with sandbox API keys before going live. The MCP server only reaches production.


## Related topics

- [Whop Plugin for Grok Build](https://docs.whop.com/developer/guides/plugins/grok.md)
- [AI Tools for Building on Whop](https://docs.whop.com/developer/ai/overview.md)
- [AI and MCP](https://docs.whop.com/developer/guides/ai_and_mcp.md)
