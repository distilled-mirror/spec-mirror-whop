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

# ChartElement

> An account’s performance over a window, with a picker for what to plot — spend, impressions, clicks, or any conversion the account records. Read-only.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Reporting`](https://docs.whop.com/elements/latest/ads/reporting), in [`Ads`](https://docs.whop.com/elements/latest/ads/overview). Pass props and callbacks through the create options or React props.

```tsx React theme={null}
import { WhopElements, Ads, Reporting, ChartElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Ads /* options */>
        <Reporting>
          <ChartElement onMetricChanged={(e) => console.log(e)} onTimezoneChanged={(e) => console.log(e)} onPeriodChanged={(e) => console.log(e)} />
        </Reporting>
      </Ads>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const ads = window.WhopElements().ads.create({ /* options */ });
  const reporting = ads.create('reporting', { /* options */ });
  reporting.create('chart', {
    onMetricChanged: (e) => console.log(e),
    onTimezoneChanged: (e) => console.log(e),
    onPeriodChanged: (e) => console.log(e)
  }).mount('#ads-reporting-chart');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/ads/overview#playground).

## Props

**metric** `string`

What the chart plots — `spend`, `impressions`, `clicks`, or a conversion. The picker moves it; set it to open on something else. For `conversions`, `conversion_value` and `cost_per_conversion`, name the conversion in `metricEvent`. Defaults to `"spend"`.

One of `"funnel"`, `"spend"`, `"impressions"`, `"clicks"`, `"cost_per_1k_impressions"`, `"cost_per_click"`, `"conversions"`, `"conversion_value"`, or `"cost_per_conversion"`.

**metricEvent** `string`

The conversion `metric` refers to when it is a per-conversion one — `payment.completed`, `pixel.lead`, or a custom event the account names itself. Ignored by every other metric. Defaults to `""`.

## Events

Pass callbacks in the create options or React props.

### `onMetricChanged`

The viewer picked a different result to plot. `metric` is what is being plotted; `event` names the conversion it refers to, and is present only for `conversions`, `conversion_value` and `cost_per_conversion` — the account decides what those are called, so it is a string rather than a member. Mirror it into your own URL and hand it back as `metric`/`metricEvent` if you want the choice to survive a reload; the element holds it for the life of the mount either way.

**payload** `object`

**properties**

**metric** `string` *required*

One of `"funnel"`, `"spend"`, `"impressions"`, `"clicks"`, `"cost_per_1k_impressions"`, `"cost_per_click"`, `"conversions"`, `"conversion_value"`, or `"cost_per_conversion"`.

**event** `string`

### `onTimezoneChanged`

The viewer switched which zone the figures are reported in: the choice they made AND the IANA zone it resolves to. The choice is the half that survives a reload — persist it and pass it back as `timezone`, which is what the Whop dashboard does with its own copy; the element holds it for the life of the mount either way. `resolved` comes along so a host can label its own figures without repeating the account-preference lookup, and is the viewer's own zone whenever the account has set none.

**payload** `object`

**properties**

**timezone** `"account" | "local"` *required*

**resolved** `string` *required*

### `onPeriodChanged`

The viewer moved the reporting window: the period they picked AND the window it resolves to, as ISO 8601 instants. The dates come resolved because a period name alone would leave you redoing the day-boundary and account-timezone arithmetic to act on it; the period comes too because only it survives a reload as a ROLLING window — stored dates would pin it. Every surface under the same handle has already followed the move.

**payload** `object`

**properties**

**period** `string` *required*

One of `"today"`, `"yesterday"`, `"last_7_days"`, `"last_14_days"`, `"last_30_days"`, `"last_90_days"`, `"all_time"`, or `"custom"`.

**from** `string` *required*

**to** `string` *required*

### `onLoaderStart`

Runs after the loading skeleton first paints and before `onReady`.

### `onReady`

Runs after the element's first complete paint.

### `onError`

Runs when the element fails to load or crashes. The fallback remains visible. Use `code` for programmatic handling. `sourceKey` identifies a failed host-state source.

**e** `object`

**properties**

**message** `string` *required*

**code** `string`

**sourceKey** `string`

## Methods

Call these on the handle returned by `create`, or through a React `ref`.

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<ChartElementProps\>` *required*

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.


## Related topics

- [Reporting](https://docs.whop.com/elements/latest/ads/reporting.md)
- [BalanceElement](https://docs.whop.com/elements/latest/wallet/balances-balance.md)
- [ActivityElement](https://docs.whop.com/elements/latest/wallet/activity.md)
