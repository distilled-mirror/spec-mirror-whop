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

# Start a Project from a Blueprint

> Start from a working Whop business. Browse the blueprint gallery, deploy one in a step, or clone its code and hand it to your AI assistant.

Start from a working business instead of a blank file. The blueprint gallery at [whop.com/blueprints](https://whop.com/blueprints) is the canonical list: complete, running businesses built on the Current API, including blueprints published by the community. Deploying one gives you your own copy — products, pricing, and a live site — with nothing to build on your machine. See [Blueprints](https://docs.whop.com/developer/websites/blueprints.md) for how deploying works and the 10% affiliate cut publishers earn.

To change how a blueprint's site works, clone its source. Every blueprint in [`whopio/templates`](https://github.com/whopio/templates) is self-contained and ships with sample data, so it runs with no credentials. You can see the whole app before wiring up an account.

```bash theme={null}
git clone https://github.com/whopio/templates.git
```

Or clone one straight from the gallery with the [CLI](https://docs.whop.com/cli/overview.md), which registers a new app on its own domain so the first `whop apps deploy` ships there:

```bash theme={null}
whop apps init --template app_xxxxxxxx --name "Shine Time" --domain shine-time
```

## App blueprints

An app that installs into a community is a different starting point from an API integration. Use [`whop-nextjs-app-template`](https://github.com/whopio/whop-nextjs-app-template), or scaffold directly with `whop apps init` from the [CLI](https://docs.whop.com/cli/overview.md).

## Going live

Two rules apply to every blueprint:

* Never ship a Whop API key in a client. Mint a short-lived access token on your own server, scoped to the acting user. See [Auth & API keys](https://docs.whop.com/developer/guides/auth-scoping.md).
* Pin the API version. Each blueprint sends an `Api-Version-Date` header. See [API versioning](https://docs.whop.com/developer/api/versioning.md).

> [!TIP]
>
> When you build with an AI assistant, point it at a blueprint plus the [Whop MCP server](https://docs.whop.com/developer/guides/ai_and_mcp.md) and it has both a working shape and the current API in context.

## Next steps

> [card] [Blueprint gallery](https://whop.com/blueprints)
>
> Browse every blueprint, including community builds.

> [card] [Deploy a blueprint](https://docs.whop.com/developer/websites/blueprints.md)
>
> Products, pricing, and a live site in one step.

> [card] [Build with the CLI and Claude](https://docs.whop.com/developer/guides/cli-with-claude.md)
>
> Scaffold from your terminal.


## Related topics

- [Website Hosting on Whop](https://docs.whop.com/developer/websites/hosting.md)
- [Build a Website with the CLI](https://docs.whop.com/developer/websites/quickstart.md)
- [Deploy a Blueprint Business](https://docs.whop.com/developer/websites/blueprints.md)
