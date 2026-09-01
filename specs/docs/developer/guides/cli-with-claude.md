> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Build with the CLI and Claude

> Use the Whop CLI together with Claude to scaffold, build, and ship a Whop app from your terminal.

The Whop CLI plus Claude turns "I want to build a Whop app" into a working integration without leaving your terminal. The CLI scaffolds, deploys, and can register itself as a tool for your agent, while Claude reads Whop's docs over MCP and writes the code. You stay in the loop.

<Note>
  You need a Whop account. Install the CLI with `curl -fsSL https://whop.com/install.sh | sh`. Commands on this page are verified against CLI v0.16.1.
</Note>

<Warning>
  The CLI operates on production. There is no sandbox mode, and its wizards can create real apps and move real money.
</Warning>

## 1. Pick your business

```bash theme={null}
whop quickstart
```

Chooses or creates the Whop business the CLI acts on, and signs you in. To sign in on its own, `whop login --method oauth` opens a browser.

## 2. Give your agent Whop's tools

Connect the two Whop MCP servers so Claude can read the docs and call the API as it builds. The install blocks for both are on [AI and MCP](/developer/guides/ai_and_mcp).

Then sync Whop's skill files, so the agent knows Whop's conventions without being told each time:

```bash theme={null}
whop skills add
```

Now Claude can answer questions like how to accept payments on Whop from the current docs, and act on your account through the API.

## 3. Scaffold an app

```bash theme={null}
whop apps init --name "My App" --app_type b2c_app
```

Registers the app, scaffolds the latest template, wires it for Whop hosting, and installs dependencies. Then ask Claude: *"Using the Whop docs, add an embedded checkout and a webhook that grants access on payment."* Claude pulls the current API from the Docs MCP and writes against it.

## 4. Develop, then ship

```bash theme={null}
whop apps dev       # build and run locally
whop apps deploy    # build, typecheck, upload, and promote to production
```

Use `whop apps deploy --preview` to upload without promoting, then promote later. That's also how you roll back.

<Tip>
  Keep Claude pointed at the Whop Docs MCP server for the whole session. It keeps the code on the current API.
</Tip>

## Next steps

<Columns cols={2}>
  <Card title="AI and MCP" icon="plug" href="/developer/guides/ai_and_mcp">
    The full MCP setup.
  </Card>

  <Card title="Whop CLI" icon="terminal" href="/cli/overview">
    The full command surface.
  </Card>
</Columns>
