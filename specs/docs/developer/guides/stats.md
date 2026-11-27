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

# Stats API

> List the metrics the Stats API offers and query an account's metrics over time, with filters and breakdowns.

Use the Stats API to discover available metrics, then retrieve a time series for an account. The [Stats reference](https://docs.whop.com/api-reference/stats/stats) explains each metric, its units, and supported filters.

> [!NOTE]
>
> Account revenue queries require the `stats:read` permission. See
> [Permissions](https://docs.whop.com/developer/guides/permissions.md) to request permissions for your app.

## Discover metrics

[List time series metrics](https://docs.whop.com/api-reference/stats/list-time-series-metrics.md) to get their keys, names, units, descriptions, and filterable properties:

```bash theme={null}
curl "https://api.whop.com/api/v1/stats/time_series" \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

Use a metric's `key`, such as `gross_revenue`, in the next request. Its `properties` array lists the properties you can filter or break down by.

## Retrieve a time series

Pass your account ID and a date range to [Retrieve metric time series](https://docs.whop.com/api-reference/stats/retrieve-metric-time-series.md):

```bash theme={null}
curl --get "https://api.whop.com/api/v1/stats/time_series/gross_revenue" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  --data-urlencode "account_id=YOUR_ACCOUNT_ID" \
  --data-urlencode "from=2026-08-01" \
  --data-urlencode "to=2026-08-31" \
  --data-urlencode "interval=day" \
  --data-urlencode "convert_to=usd"
```

The response's `data.points` array contains a Unix `timestamp` in seconds and a `value` for each period. Read the value in the catalog's unit: `count` is a count, `currency` is a decimal amount, and `percent` is already in percentage points (`1.6` means 1.6%).

Some metrics also return `data.totals` for the whole range. Use those totals when provided: averaging daily rates or summing daily unique counts doesn't give the whole-range result.

## Filter and break down results

Pass a property directly as a query parameter to filter it. Use `breakdown_by` to split each point by one property. This example filters gross revenue to card payments and splits it by the original transaction currency:

```bash theme={null}
curl --get "https://api.whop.com/api/v1/stats/time_series/gross_revenue" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  --data-urlencode "account_id=YOUR_ACCOUNT_ID" \
  --data-urlencode "from=2026-08-01" \
  --data-urlencode "to=2026-08-31" \
  --data-urlencode "payment_method=card" \
  --data-urlencode "breakdown_by=currency"
```

Each point includes a `breakdown` array of `{ name, value }` entries. Filtering or breaking down transaction metrics by `currency` reports their original currency amounts without conversion.

Only use properties listed for the metric. An unsupported property returns `400`. An unknown metric key returns `404`. See the [query parameters](https://docs.whop.com/api-reference/stats/stats#query-parameters) and [metric reference](https://docs.whop.com/api-reference/stats/retrieve-metric-time-series.md) for time zones, intervals, snapshot windows, and metric-specific options.


## Related topics

- [Stats API Reference](https://docs.whop.com/api-reference/stats/stats.md)
- [Get Experiment Results](https://docs.whop.com/api-reference/experiments/get-experiment-results.md)
- [Create Experiment](https://docs.whop.com/api-reference/experiments/create-experiment.md)
