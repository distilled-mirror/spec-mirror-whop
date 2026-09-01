> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# AI Tools

> MCP servers, an installable skill, an LLM page index, and raw Markdown for every page: what Whop gives a coding agent.

Whop's docs and API are built to be read by AI agents as well as people. Which surface you want depends on whether the agent needs to **read** Whop or **act on** it.

## Pick a surface

| You want an agent to                                 | Use                        |
| ---------------------------------------------------- | -------------------------- |
| Answer questions from the docs while it writes code  | The Whop Docs MCP server   |
| Call the Current API on your behalf                  | The Whop MCP server        |
| Know Whop's conventions without being told each time | The skill                  |
| Read the docs with no MCP client at all              | `llms.txt` or raw Markdown |
| Scaffold, build, and ship a Whop app from a terminal | The CLI in agent mode      |

## MCP servers

Whop runs two, and they do different jobs:

* **The Whop MCP server** at `mcp.whop.com/mcp` operates on live Whop data. It connects through a browser sign-in, so you never put an API key in the client config.
* **The Whop Docs MCP server** at `docs.whop.com/mcp` searches and reads this documentation.

[AI and MCP](/developer/guides/ai_and_mcp) has the connection details and per-client setup.

## The skill

Whop's docs publish an installable agent skill at [`docs.whop.com/.well-known/skills/index.json`](https://docs.whop.com/.well-known/skills/index.json). Each entry's `SKILL.md` teaches an agent what Whop is, when to reach for it, and where the API and SDKs live.

Agents that support the skills convention discover the catalog on their own. For agents that don't, the Whop CLI syncs the same material with `whop skills add`, covered in [Agent Mode](/cli/agent-mode).

## Plain files for agents

You don't need an MCP client to give an agent good context.

* **[llms.txt](https://docs.whop.com/llms.txt)** indexes every page by product area with a one-line description each, and steers agents toward the current API.
* **[llms-full.txt](https://docs.whop.com/llms-full.txt)** is the same corpus expanded, for agents that want everything in one fetch.
* **Append `.md` to any docs URL** to get the raw Markdown of that page. `docs.whop.com/developer/quickstart.md` returns the source of the quickstart, which is what you want in a context window instead of scraped HTML.

## The CLI as an agent tool

The Whop CLI emits machine-readable output on every command, so an agent can call it directly and parse the result. That covers what the API doesn't: scaffolding a project, running it locally, and deploying. [Agent Mode](/cli/agent-mode) covers the flags. [Build with the CLI and Claude](/developer/guides/cli-with-claude) walks through it end to end.

## Where to go next

<Columns cols={2}>
  <Card title="Connect an MCP server" icon="plug" href="/developer/guides/ai_and_mcp">
    Setup for the docs and API servers.
  </Card>

  <Card title="Build with the CLI and Claude" icon="terminal" href="/developer/guides/cli-with-claude">
    Scaffold and ship an app end to end.
  </Card>

  <Card title="Agent Mode" icon="robot" href="/cli/agent-mode">
    Skills sync and the machine-readable output flags.
  </Card>

  <Card title="Quickstart" icon="rocket" href="/developer/quickstart">
    The first API call, for you, or for your agent.
  </Card>
</Columns>
