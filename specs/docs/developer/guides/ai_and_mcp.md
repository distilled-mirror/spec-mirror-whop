> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# AI and MCP

> Per-client setup for both Whop MCP servers, browser sign-in, confirmation and idempotency, plus the LLM page index and raw Markdown for agents with no MCP client.

Whop runs two Model Context Protocol (MCP) servers: one that lets agents search and read these docs, and one that lets them operate on live Whop data.

<Tip>
  Claude Code and Grok have a Whop plugin that installs this server for you.
  See [Claude Code](/developer/guides/plugins/claude-code) and [Grok](/developer/guides/plugins/grok).
</Tip>

## Whop Docs MCP server

Gives AI agents direct access to Whop documentation for accurate guidance while building. Server URL: `https://docs.whop.com/mcp`

<CodeGroup>
  ```bash Claude Code theme={null}
  claude mcp add --transport http whop-docs https://docs.whop.com/mcp
  ```

  ```json Cursor theme={null}
  // Global: ~/.cursor/mcp.json (or per project: .cursor/mcp.json)
  {
  	"mcpServers": {
  		"whop-docs": {
  			"type": "http",
  			"url": "https://docs.whop.com/mcp"
  		}
  	}
  }
  ```
</CodeGroup>

One-click install of the docs server for Cursor:

[![Install MCP Server](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/en-US/install-mcp?name=whop-docs\&config=eyJ1cmwiOiJodHRwczovL2RvY3Mud2hvcC5jb20vbWNwIn0%3D)

## Whop MCP server

Lets AI agents call the Current API: list resources, create data, and explore your setup interactively. Server URL: `https://mcp.whop.com/mcp`

<CodeGroup>
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
</CodeGroup>

### One URL, every client

Clients on older protocol revisions connect to the same `/mcp` URL with no special configuration. `https://mcp.whop.com/sse` stays supported for clients already pointed at it. Configure new clients with `/mcp`.

### Authentication

The server signs you in through the browser, so the whole client config is a URL. Don't add an API key, a bearer token, or a custom authorization header. On first use your browser opens Whop's sign-in screen, which lists the scopes the connection requests. Whop holds the credentials server-side. They never reach your client config or the model.

### What a connection can reach

The grant is you, not one business. Business context is a parameter on each operation, so the agent chooses it per call. The tool surface is a reviewed subset of the API. Endpoints that need a first-party whop.com session are excluded.

<Warning>
  Confirmation and idempotency reduce accidental or duplicate actions. They don't reduce the privileges of the grant. Connect only MCP clients you trust, inspect the actions an agent proposes, and disconnect access when you no longer need it.
</Warning>

### Confirming consequential operations

Consequential operations run in two calls. The first returns `prepared: true` with the effect, a signed `mcp_confirmation_token`, and its expiry. Nothing has executed at that point. Call the same tool again with the same arguments plus the token and a stable `idempotency_key` to execute. If the arguments change between the two calls, the server returns `confirmation_invalid`.

### Retries and idempotency

The `idempotency_key` is a stable key you pick so a retry replays instead of re-executing. On an uncertain outcome the server's response says whether to retry with the same key or verify the effect first. Never switch to a new key until you've verified what happened.

### Connect from Claude.ai

1. Open [Claude](https://claude.ai).
2. Open **Settings**, then **Connectors**.
3. Select **Add custom connector**.
4. Enter the server URL: `https://mcp.whop.com/mcp`

Disconnect the same way: open the connector and remove it. The grant stops working immediately.

### When sign-in fails

| What you see                              | What happened                                                                              |
| ----------------------------------------- | ------------------------------------------------------------------------------------------ |
| This sign-in attempt expired. Start over. | The attempt sat too long. Start the connection again.                                      |
| A 403 at the end of sign-in               | Sign-in started in one browser and finished in another. Run the whole flow in one browser. |
| `invalid_grant`                           | The connection's Whop credential is no longer valid. Reconnect to re-authorize.            |

## Plain-file access for agents

If you have no MCP client, [docs.whop.com/llms.txt](https://docs.whop.com/llms.txt) indexes every page by product area, and appending `.md` to any docs URL returns the raw Markdown.

## Next steps

<CardGroup cols={2}>
  <Card title="API overview" icon="map" href="/api-reference/beta/overview">
    How requests, versioning, and pagination work.
  </Card>

  <Card title="Quickstart" icon="rocket" href="/developer/quickstart">
    Make your first API call in a few minutes.
  </Card>

  <Card title="Test in sandbox" icon="flask" href="/developer/guides/sandbox">
    Point your agent at test data before going live.
  </Card>

  <Card title="Whop CLI" icon="terminal" href="/cli/overview">
    Build and manage Whop apps from your terminal.
  </Card>
</CardGroup>
