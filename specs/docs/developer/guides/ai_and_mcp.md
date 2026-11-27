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

# AI and MCP

> Connect AI agents to the Whop MCP server to call the API and search the docs, with browser sign-in and idempotent retries.

The Whop Model Context Protocol (MCP) server lets AI agents operate on live Whop data and search these docs from one connection.

> [!TIP]
>
> Claude Code and Grok have a Whop plugin that installs this server for you.
> See [Claude Code](https://docs.whop.com/developer/guides/plugins/claude-code.md) and [Grok](https://docs.whop.com/developer/guides/plugins/grok.md).

## Whop MCP server

Lets AI agents call the Current API: list resources, create data, and explore your setup interactively. It also searches and reads this documentation, so agents look up the current API while they build. Server URL: `https://mcp.whop.com/mcp`

```bash Claude Code theme={null}
claude mcp add --transport http whop-api https://mcp.whop.com/mcp
```

```json Cursor theme={null}
// Global: ~/.cursor/mcp.json (or per project: .cursor/mcp.json)
{
	"mcpServers": {
		"whop": {
			"type": "http",
			"url": "https://mcp.whop.com/mcp"
		}
	}
}
```

### One URL, every client

All clients connect to the `/mcp` URL. Clients on older protocol revisions use the same URL with no special configuration.

> [!NOTE]
>
> If you connected using the `/sse` URL, update it to `/mcp`. The `/sse` endpoint has been retired and now returns a 404.

### Authentication

The server signs you in through the browser, so the whole client config is a URL. Don't add an API key, a bearer token, or a custom authorization header. On first use your browser opens Whop's sign-in screen, which lists the scopes the connection requests. Whop holds the credentials server-side. They never reach your client config or the model.

### What a connection can reach

The grant is you, not one business. Business context is a parameter on each operation, so the agent chooses it per call. The tool surface is a reviewed subset of the API. Endpoints that need a first-party whop.com session are excluded.

> [!WARNING]
>
> Every tool call runs when the agent makes it, so your MCP client decides what you approve. Payment, credential, and destructive tools carry `destructiveHint`, which clients use to ask you first. Connect only MCP clients you trust, inspect the actions an agent proposes, and disconnect access when you no longer need it.

### Retries and idempotency

The `idempotency_key` is a stable key you pick so a retry replays instead of re-executing. The server sends it to the API as the `Idempotency-Key` header. After an uncertain outcome, retry with the same key. Never switch to a new key until you've verified what happened.

### Connect from Claude.ai

1. Open [Claude](https://claude.ai).
2. Open **Settings**, then **Connectors**.
3. Select **Add custom connector**.
4. Enter the server URL: `https://mcp.whop.com/mcp`

Disconnect the same way: open the connector and remove it. The grant stops working immediately.

### When sign-in fails

| What you see | What happened |
| - | - |
| This sign-in attempt expired. Start over. | The attempt sat too long. Start the connection again. |
| A 403 at the end of sign-in | Sign-in started in one browser and finished in another. Run the whole flow in one browser. |
| `invalid_grant` | The connection's Whop credential is no longer valid. Reconnect to re-authorize. |

## Plain-file access for agents

If you have no MCP client, [docs.whop.com/llms.txt](https://docs.whop.com/llms.txt) indexes every page the way the docs navigation groups them, and appending `.md` to any docs URL returns the raw Markdown.

## Next steps

> [card] [API overview](https://docs.whop.com/api-reference/overview.md)
>
> How requests, versioning, and pagination work.

> [card] [Quickstart](https://docs.whop.com/developer/quickstart.md)
>
> Make your first API call in a few minutes.

> [card] [Test in sandbox](https://docs.whop.com/developer/guides/sandbox.md)
>
> Test with sandbox API keys before going live. The MCP server only reaches production.

> [card] [Whop CLI](https://docs.whop.com/cli/overview.md)
>
> Build and manage Whop apps from your terminal.


## Related topics

- [AI Tools for Building on Whop](https://docs.whop.com/developer/ai/overview.md)
- [Build with the CLI and Claude](https://docs.whop.com/developer/guides/cli-with-claude.md)
- [Whop Plugin for Claude Code](https://docs.whop.com/developer/guides/plugins/claude-code.md)
