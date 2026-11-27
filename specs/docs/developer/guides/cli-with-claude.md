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

# Build with the CLI and Claude

> Use the Whop CLI together with Claude to scaffold, build, and ship a Whop app from your terminal.

The Whop CLI plus Claude turns "I want to build a Whop app" into a working integration without leaving your terminal. The CLI scaffolds, deploys, and can register itself as a tool for your agent, while Claude reads Whop's docs over MCP and writes the code. You stay in the loop.

> [!NOTE]
>
> You need a Whop account. Install the CLI with `curl -fsSL https://whop.com/install.sh | sh`. Commands on this page are verified against CLI v0.16.1.

> [!WARNING]
>
> The CLI operates on production. There is no sandbox mode, and its wizards can create real apps and move real money.

## 1. Pick your business

```bash theme={null}
whop quickstart
```

Chooses or creates the Whop business the CLI acts on, and signs you in. To sign in on its own, `whop login --method oauth` opens a browser.

## 2. Give your agent Whop's tools

Connect the two Whop MCP servers so Claude can read the docs and call the API as it builds. The install blocks for both are on [AI and MCP](https://docs.whop.com/developer/guides/ai_and_mcp.md).

Then sync Whop's skill files, so the agent knows Whop's conventions without being told each time:

```bash theme={null}
whop skills add
```

Now Claude can answer questions like how to accept payments on Whop from the current docs, and act on your account through the API.

## 3. Scaffold an app

```bash theme={null}
whop apps init --name "My App" --app_type b2c_app
```

Registers the app, scaffolds the latest template, wires it for Whop hosting, and installs dependencies. Then ask Claude: *"Using the Whop docs, add a Whop Elements checkout and a webhook that grants access on payment."* Claude pulls the current API from the [Whop MCP server](https://docs.whop.com/developer/guides/ai_and_mcp.md) and writes against it.

## 4. Develop, then ship

```bash theme={null}
whop apps dev       # build and run locally
whop apps deploy    # build, typecheck, upload, and promote to production
```

Use `whop apps deploy --preview` to upload without promoting, then promote later. That's also how you roll back.

> [!TIP]
>
> Keep Claude connected to the Whop MCP server for the whole session. It keeps the code on the current API.

## Next steps

> [card] [AI and MCP](https://docs.whop.com/developer/guides/ai_and_mcp.md)
>
> The full MCP setup.

> [card] [Whop CLI](https://docs.whop.com/cli/overview.md)
>
> The full command surface.


## Related topics

- [Whop CLI](https://docs.whop.com/cli/overview.md)
- [Whop CLI Agent Mode](https://docs.whop.com/cli/agent-mode.md)
- [AI Tools for Building on Whop](https://docs.whop.com/developer/ai/overview.md)
