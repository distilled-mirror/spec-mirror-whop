> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Stats API

> Discover metrics and query revenue, payments, and engagement over time

Use the Stats API to discover available metrics, then retrieve a time series for an account. The [Stats reference](/api-reference/beta/stats/stats) explains each metric, its units, and supported filters.

<Note>
  Account revenue queries require the `stats:read` permission. See
  [Permissions](/developer/guides/permissions) to request permissions for your app.
</Note>

## Discover metrics

[List metrics](/api-reference/beta/stats/list-metrics) to get their keys, names, units, descriptions, and filterable properties:

```bash theme={null}
curl "https://api.whop.com/api/v1/stats" \
  -H "Authorization: Bearer $WHOP_API_KEY"
```

Use a metric's `key`, such as `gross_revenue`, in the next request. Its `properties` array lists the properties you can filter or break down by.

## Retrieve a time series

Pass your account ID and a date range to [Retrieve metric](/api-reference/beta/stats/retrieve-metric):

```bash theme={null}
curl --get "https://api.whop.com/api/v1/stats/gross_revenue" \
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
curl --get "https://api.whop.com/api/v1/stats/gross_revenue" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  --data-urlencode "account_id=YOUR_ACCOUNT_ID" \
  --data-urlencode "from=2026-08-01" \
  --data-urlencode "to=2026-08-31" \
  --data-urlencode "payment_method=card" \
  --data-urlencode "breakdown_by=currency"
```

Each point includes a `breakdown` array of `{ name, value }` entries. Filtering or breaking down transaction metrics by `currency` reports their original currency amounts without conversion.

Only use properties listed for the metric. An unsupported property returns `400`. An unknown metric key returns `404`. See the [query parameters](/api-reference/beta/stats/stats#query-parameters) and [metric reference](/api-reference/beta/stats/retrieve-metric) for time zones, intervals, snapshot windows, and metric-specific options.
