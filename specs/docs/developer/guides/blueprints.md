> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Blueprints

> Start from a working Whop business. Browse the blueprint gallery, deploy one in a step, or clone its code and hand it to your AI assistant.

Start from a working business instead of a blank file. The blueprint gallery at [whop.com/blueprints](https://whop.com/blueprints) is the canonical list: complete, running businesses built on the Current API, including blueprints published by the community. Deploying one gives you your own copy — products, pricing, and a live site — with nothing to build on your machine. See [Blueprints](/developer/websites/blueprints) for how deploying works and the 10% affiliate cut publishers earn.

To change how a blueprint's site works, clone its source. Every blueprint in [`whopio/templates`](https://github.com/whopio/templates) is self-contained and ships with sample data, so it runs with no credentials. You can see the whole app before wiring up an account.

```bash theme={null}
git clone https://github.com/whopio/templates.git
```

Or clone one straight from the gallery with the [CLI](/cli/overview), which registers a new app so the first `whop apps deploy` ships to your route:

```bash theme={null}
whop apps init --template app_xxxxxxxx --name "Shine Time" --route shine-time
```

## App blueprints

An app that installs into a community is a different starting point from an API integration. Use [`whop-nextjs-app-template`](https://github.com/whopio/whop-nextjs-app-template), or scaffold directly with `whop apps init` from the [CLI](/cli/overview).

## Going live

Two rules apply to every blueprint:

* Never ship a Whop API key in a client. Mint a short-lived access token on your own server, scoped to the acting user. See [Auth & API keys](/developer/guides/auth-scoping).
* Pin the API version. Each blueprint sends an `Api-Version-Date` header. See [API versioning](/developer/api/versioning).

<Tip>
  When you build with an AI assistant, point it at a blueprint plus the [Docs MCP server](/developer/guides/ai_and_mcp) and it has both a working shape and the current API in context.
</Tip>

## Next steps

<Columns cols={2}>
  <Card title="Blueprint gallery" icon="store" href="https://whop.com/blueprints">
    Browse every blueprint, including community builds.
  </Card>

  <Card title="Deploy a blueprint" icon="rocket" href="/developer/websites/blueprints">
    Products, pricing, and a live site in one step.
  </Card>

  <Card title="Build with the CLI and Claude" icon="terminal" href="/developer/guides/cli-with-claude">
    Scaffold from your terminal.
  </Card>
</Columns>
