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

# Scheduled Runs

> Run your hosted app's code on a cron, either once per slot or once for every installation.

A scheduled run calls your hosted app on a cron instead of in response to a request. Use one for work that no Whop event triggers, such as pulling new orders from another system every 15 minutes or reconciling payouts overnight.

## Set a schedule

An app has two schedules. Set either or both with [Update app](https://docs.whop.com/api-reference/apps/update-app.md):

```bash theme={null}
curl -X PATCH "https://api.whop.com/api/v1/apps/app_XXXXXXXX" \
  -H "Authorization: Bearer $WHOP_DEVELOPER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "schedule": "0 3 * * *",
    "per_install_schedule": "*/15 * * * *"
  }'
```

| Field | Runs |
| - | - |
| `schedule` | Once per slot, for the app as a whole. |
| `per_install_schedule` | Once per slot for every account that has the app installed. Each run carries that installation. Can't fire more often than every 15 minutes. |

Both take a five-field cron expression, read in UTC, of at most 64 characters. The expression has to fire at least once. `schedule` has no minimum interval. Send `null` or an empty string to clear a schedule. An invalid expression returns a `400`. The key needs `developer:update_app` on the account that owns the app.

## Handle the run

Export `scheduled()` next to `fetch()` in your build's server entry:

```typescript src/server.ts theme={null}
export default {
  async fetch(request, env, ctx) {
    /* ... */
  },

  async scheduled(controller, env, ctx) {
    const { kind, app_installation } = ctx.props;

    if (kind === "global") {
      await reconcilePayouts();
      return;
    }

    await syncOrders(app_installation.external_id, app_installation.metadata.orders_cursor);
  },
};
```

`ctx.props` says which run this is:

| Prop | Value |
| - | - |
| `kind` | `global` for a `schedule` run, `install` for a `per_install_schedule` run. |
| `app_installation` | The installation, as the [app installation](https://docs.whop.com/api-reference/app-installations/app-installation) object. `null` on a `global` run. |
| `run_id` | ID of this slot's run. Every installation's run in the same slot shares it. |
| `scheduled_at` | The slot the run is for, as an ISO 8601 timestamp. |
| `attempt` | `1` on the first try, `2` on the retry. |

`controller.cron` is the expression that scheduled the run, and `controller.scheduledTime` is the slot in milliseconds since the epoch.

A run uses the same isolate as your request handlers, so secrets and the [injected API key](https://docs.whop.com/developer/websites/hosting.md#call-the-whop-api) work the same way. A run can last up to 15 minutes, where a request handler gets 30 seconds.

> [!TIP]
>
> Keep a per-installation sync cursor in the installation's `metadata`, as described in [App installations](https://docs.whop.com/developer/guides/app-installations.md). Each run reads it from `ctx.props.app_installation.metadata` and writes the new value back when it finishes.

## Retries and repeated runs

If a run fails, Whop retries it one time, a minute later, then drops it. To skip the retry, call `controller.noRetry()` before you throw. On the retry, `attempt` is `2` and `scheduled_at` is still the original slot.

The same slot can reach your app more than once, and a retry can start while the next slot's run is already going. If your work must not happen twice, record each run you finish under `run_id` plus `app_installation.id`, and skip a run you've already recorded.

## Skipped slots

No run happens while the app has no production web build with a server entry. Whop doesn't queue a slot it skipped or make it up later, so build each run to carry on from whatever the last one saved.

## Logs

A run's `console` output and uncaught errors appear in your app's [logs](https://docs.whop.com/developer/websites/hosting.md#logs), next to its requests.

## Next steps

> [card] [App installations](https://docs.whop.com/developer/guides/app-installations.md)
>
> The installation each per-install run carries, and where to keep a sync cursor.

> [card] [Hosting](https://docs.whop.com/developer/websites/hosting.md)
>
> Builds, secrets, the injected API key, and logs.


## Related topics

- [Website Hosting on Whop](https://docs.whop.com/developer/websites/hosting.md)
- [Run a Bounty Program](https://docs.whop.com/developer/bounties/run-a-bounty-program.md)
- [Cancel Scheduled Send](https://docs.whop.com/api-reference/invoices/cancel-scheduled-send.md)
