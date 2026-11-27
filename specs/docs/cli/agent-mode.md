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

# Whop CLI Agent Mode

> Sync Whop skills into your agent and use the CLI's machine-readable output flags.

AI agents can drive the whole Whop CLI. There is no separate `agent` command. Agent mode is a set of flags and integrations on the CLI itself.

> [!WARNING]
>
> The CLI has no sandbox, test, or dry-run mode. An agent driving it operates on production.

## Giving an agent Whop's tools

Connect the [Whop MCP server](https://docs.whop.com/developer/guides/ai_and_mcp.md) at `https://mcp.whop.com/mcp`. It signs in through the browser, needs no API key in the client config, and asks before it runs consequential operations.

Use the CLI alongside it for the things the API doesn't do: scaffolding a project, running a dev server, and deploying.

`whop mcp add` registers the CLI itself as a local MCP server instead. Its tools are the CLI's commands, except sign-in: `auth`, `login`, and `logout` aren't exposed, so run `whop login` or set `WHOP_API_KEY` before the agent starts.

## Skills

`whop skills add` syncs Whop's skill files into your agent: one `whop` skill plus playbook references that teach the agent Whop's payment, app, and ads flows. `whop skills list` shows what's installed.

## Machine-readable output

Global flags on every command, so an agent can call the CLI directly and parse what comes back:

| Flag | Purpose |
| - | - |
| `--llms`, `--llms-full` | Print an LLM-readable manifest of the whole CLI |
| `--schema` | JSON Schema for a command |
| `--format <toon\|json\|yaml\|md\|jsonl>` | Output format |
| `--filter-output <keys>` | Filter output by key paths |
| `--token-count` / `--token-limit <n>` / `--token-offset <n>` | Token-budget the output |

## Signing an agent in

For unattended use, pass `--api-key` or set `WHOP_API_KEY`. For OAuth, follow the CLI's own agent guidance. Run `whop login --method oauth --format jsonl` in the background with output to a file. Open the emitted `authorizationUrl` in the user's browser without truncating it, then wait for the final `done` event.

## Next steps

> [card] [Build with the CLI and Claude](https://docs.whop.com/developer/guides/cli-with-claude.md)
>
> The full agent-in-the-loop workflow.

> [card] [AI and MCP](https://docs.whop.com/developer/guides/ai_and_mcp.md)
>
> Connect the Whop MCP server.


## Related topics

- [Whop CLI](https://docs.whop.com/cli/overview.md)
- [Whop CLI Commands](https://docs.whop.com/cli/commands.md)
- [AI Tools for Building on Whop](https://docs.whop.com/developer/ai/overview.md)
