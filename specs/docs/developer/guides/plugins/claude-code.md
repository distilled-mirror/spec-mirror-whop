> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Claude Code

> Install the Whop plugin for Claude Code to run your business from the terminal — products, checkout, payments, payouts, ads, and stats.

The Whop plugin connects Claude Code to your live Whop business, so you can create products, take payments, run ads, and read your numbers.

The plugin bundles the [MCP server for the Whop API](/developer/guides/ai_and_mcp) with skills
that teach Claude how to use it.

## Setup

<Steps>
  <Step title="Add the marketplace">
    ```
    /plugin marketplace add whopio/plugins
    ```
  </Step>

  <Step title="Install the plugin">
    ```
    /plugin install whop@whop
    ```
  </Step>

  <Step title="Restart Claude Code">
    Claude Code loads a newly installed plugin on the next start.
  </Step>

  <Step title="Authorize">
    Run `/mcp`, select `whop`, and authenticate. Your browser opens to Whop to sign in
    and approve access.
  </Step>

  <Step title="Confirm the connection">
    ```
    /whop:whop-connect
    ```

    Claude reports who you are signed in as, which businesses the grant can reach, and
    how many tools are available.
  </Step>
</Steps>

## Using it

Explain what you want to do. Claude picks the right tool and passes your business id:

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

Ask Claude to issue a refund and you get the operation, the target, and whether it's
reversible — then it stops and waits.

<Warning>
  The plugin requests the `admin` permission profile, which covers every business your
  Whop user manages. Disconnect it from `/mcp` if you no longer need it.
</Warning>

## Next steps

<CardGroup cols={2}>
  <Card title="Grok" icon="robot" href="/developer/guides/plugins/grok">
    The same plugin for Grok Build.
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
