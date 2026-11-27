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

# AI Tools for Building on Whop

> An MCP server, an installable skill, an LLM page index, and raw Markdown for every page: what Whop gives a coding agent.

Whop's docs and API are built to be read by AI agents as well as people. Which surface you want depends on whether the agent needs to **read** Whop or **act on** it.

## Pick a surface

| You want an agent to | Use |
| - | - |
| Answer questions from the docs while it writes code, or call the Current API on your behalf | The Whop MCP server |
| Run your business from Claude Code or Grok Build | The Whop plugin |
| Know Whop's conventions without being told each time | The skill |
| Read the docs with no MCP client at all | `llms.txt` or raw Markdown |
| Scaffold, build, and ship a Whop app from a terminal | The CLI in agent mode |

## MCP server

The Whop MCP server at `mcp.whop.com/mcp` operates on live Whop data and searches and reads this documentation. It acts on production only and can't reach the sandbox, so test an integration by calling the [sandbox](https://docs.whop.com/developer/guides/sandbox.md) API directly. It connects through a browser sign-in, so you never put an API key in the client config. One connection covers both, so there is no separate docs server to install.

[AI and MCP](https://docs.whop.com/developer/guides/ai_and_mcp.md) has the connection details and per-client setup.

## Plugins

The Whop plugin bundles the Whop MCP server with skills that teach the agent how to use it.

```text Claude Code theme={null}
/plugin marketplace add whopio/plugins
/plugin install whop@whop
```

```bash Grok Build theme={null}
grok plugin install whopio/plugins#dist/grok/whop
```

[Claude Code](https://docs.whop.com/developer/guides/plugins/claude-code.md) and [Grok](https://docs.whop.com/developer/guides/plugins/grok.md) cover setup.

## The skill

Whop's docs publish an installable agent skill at [`docs.whop.com/skill.md`](https://docs.whop.com/skill.md). It teaches an agent what Whop is, when to reach for it, and where the API and SDKs live. Install it with the skills CLI:

```bash theme={null}
npx skills add https://docs.whop.com
```

Agents that support the skills convention discover it on their own through `docs.whop.com/.well-known/agent-skills/index.json`. The Whop CLI also ships its own skill for driving the CLI, installed with `whop skills add` and covered in [Agent Mode](https://docs.whop.com/cli/agent-mode.md).

## Plain files for agents

You don't need an MCP client to give an agent good context.

* **[llms.txt](https://docs.whop.com/llms.txt)** indexes every page the way the docs navigation groups them, with a one-line description each. Large sections link to their own index files. Its agent instructions point to the MCP server, the CLI, and the Current API.
* **[llms-full.txt](https://docs.whop.com/llms-full.txt)** is the same corpus expanded, for agents that want everything in one fetch.
* **Append `.md` to any docs URL** to get the raw Markdown of that page. `docs.whop.com/developer/quickstart.md` returns the source of the quickstart, which is what you want in a context window instead of scraped HTML.

## The CLI as an agent tool

The Whop CLI emits machine-readable output on every command, so an agent can call it directly and parse the result. That covers what the API doesn't: scaffolding a project, running it locally, and deploying. [Agent Mode](https://docs.whop.com/cli/agent-mode.md) covers the flags. [Build with the CLI and Claude](https://docs.whop.com/developer/guides/cli-with-claude.md) walks through it end to end.

## Where to go next

> [card] [Connect an MCP server](https://docs.whop.com/developer/guides/ai_and_mcp.md)
>
> Setup for Claude, Cursor, and other MCP clients.

> [card] [Build with the CLI and Claude](https://docs.whop.com/developer/guides/cli-with-claude.md)
>
> Scaffold and ship an app end to end.

> [card] [Agent Mode](https://docs.whop.com/cli/agent-mode.md)
>
> Skills sync and the machine-readable output flags.

> [card] [Quickstart](https://docs.whop.com/developer/quickstart.md)
>
> The first API call, for you, or for your agent.


## Related topics

- [AI and MCP](https://docs.whop.com/developer/guides/ai_and_mcp.md)
- [What You Can Build with Whop](https://docs.whop.com/developer/start.md)
- [Whop Plugin for Grok Build](https://docs.whop.com/developer/guides/plugins/grok.md)
