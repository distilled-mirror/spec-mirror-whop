> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Grok

> Install the Whop plugin for Grok Build to run your business from the terminal — products, checkout, payments, payouts, ads, and stats.

The Whop plugin connects Grok Build to your live Whop business, so you can create products, take payments, run ads, and read your numbers.

The plugin bundles the [MCP server for the Whop API](/developer/guides/ai_and_mcp) with skills
that teach Grok how to use it.

## Setup

<Steps>
  <Step title="Install the plugin">
    Find `whop` in the built-in marketplace, or install it directly:

    ```bash theme={null}
    grok plugin install whopio/plugins#dist/grok/whop
    ```
  </Step>

  <Step title="Authorize">
    Open `/mcps`, select `whop`, and press `i`. Your browser opens to Whop to sign in
    and approve access.
  </Step>

  <Step title="Confirm the connection">
    ```
    /whop-connect
    ```

    Grok reports who you are signed in as, which businesses the grant can reach, and
    how many tools are available.
  </Step>
</Steps>

If a Whop tool is missing, `grok mcp doctor whop` reports the server's configuration and
connectivity.

## Using it

Explain what you want to do. Grok picks the right tool and passes your business id:

```
List my products and this month's revenue
Create a $24 one-time product called "Dark Chocolate Sampler"
Which memberships churned last week?
Draft a Meta ad campaign for my store, but keep it as a draft
```

If you manage more than one business, say which one.

## Anything that moves money asks first

Financial, credential, and destructive operations run in two steps. The first call
**prepares** the operation and returns a preview. Nothing happens until you approve it.

Ask Grok to issue a refund and you get the operation, the target, and whether it's
reversible — then it stops and waits.

<Warning>
  The plugin requests the `admin` permission profile, which covers every business your
  Whop user manages. Revoke it from `/mcps` if you no longer need it.
</Warning>

## Next steps

<CardGroup cols={2}>
  <Card title="Claude Code" icon="terminal" href="/developer/guides/plugins/claude-code">
    The same plugin for Claude Code.
  </Card>

  <Card title="AI and MCP" icon="plug" href="/developer/guides/ai_and_mcp">
    Connect any MCP client, or the docs server.
  </Card>

  <Card title="API overview" icon="map" href="/api-reference/beta/overview">
    How requests, versioning, and pagination work.
  </Card>

  <Card title="Test in sandbox" icon="flask" href="/developer/guides/sandbox">
    Point your agent at test data before going live.
  </Card>
</CardGroup>
