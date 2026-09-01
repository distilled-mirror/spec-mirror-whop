> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agent Mode

> Sync Whop skills into your agent and use the CLI's machine-readable output flags.

AI agents can drive the whole Whop CLI. There is no separate `agent` command. Agent mode is a set of flags and integrations on the CLI itself.

<Warning>
  The CLI has no sandbox, test, or dry-run mode. An agent driving it operates on production.
</Warning>

## Giving an agent Whop's tools

Connect the [Whop MCP server](/developer/guides/ai_and_mcp) at `https://mcp.whop.com/mcp`. It signs in through the browser, needs no API key in the client config, and asks before it runs consequential operations.

Use the CLI alongside it for the things the API doesn't do: scaffolding a project, running a dev server, and deploying.

## Skills

`whop skills add` syncs Whop's skill files into your agent: one `whop` skill plus playbook references that teach the agent Whop's payment, app, and ads flows. `whop skills list` shows what's installed.

## Machine-readable output

Global flags on every command, so an agent can call the CLI directly and parse what comes back:

| Flag                                                         | Purpose                                         |
| ------------------------------------------------------------ | ----------------------------------------------- |
| `--llms`, `--llms-full`                                      | Print an LLM-readable manifest of the whole CLI |
| `--schema`                                                   | JSON Schema for a command                       |
| `--format <toon\|json\|yaml\|md\|jsonl>`                     | Output format                                   |
| `--filter-output <keys>`                                     | Filter output by key paths                      |
| `--token-count` / `--token-limit <n>` / `--token-offset <n>` | Token-budget the output                         |

## Signing an agent in

For unattended use, pass `--api-key` or set `WHOP_API_KEY`. For OAuth, follow the CLI's own agent guidance. Run `whop login --method oauth --format jsonl` in the background with output to a file. Open the emitted `authorizationUrl` in the user's browser without truncating it, then wait for the final `done` event.

## Next steps

<Columns cols={2}>
  <Card title="Build with the CLI and Claude" icon="wand-magic-sparkles" href="/developer/guides/cli-with-claude">
    The full agent-in-the-loop workflow.
  </Card>

  <Card title="AI and MCP" icon="robot" href="/developer/guides/ai_and_mcp">
    Connect the Whop MCP server.
  </Card>
</Columns>
