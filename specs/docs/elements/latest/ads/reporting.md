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

# Reporting

> An advertising account's reporting: what it spent and what came back, over a window, at whatever level you narrow to. Mount `chart`, `table`, or both — they read the same window, the same credit rule, and the same selection, so two surfaces side by side can never disagree about what they are showing. The `ads` handle's `links` configuration wires the table's click and result counts to your own pages — the ones where you mounted the `tracking` namespace's `people` and `events`.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Ads`](https://docs.whop.com/elements/latest/ads/overview). Create it to get a handle, then mount its elements on that handle. Call `destroy()` to remove the sub-controller. Create it again to get a fresh handle.

## Preview

A live, interactive demo of this sub-controller's default arrangement with example data:

```tsx React theme={null}
import { WhopElements, Ads, Reporting, ChartElement, TableElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Ads /* options */>
        <Reporting>
          <ChartElement />
          <TableElement />
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
  reporting.create('chart').mount('#ads-reporting-chart');
  reporting.create('table').mount('#ads-reporting-table');
</script>
```

## Options

Pass these to `ads.create('reporting', { … })`, or as props on `<Reporting>` in React. Parent-injected props never appear here.

**period** `string`

The window every surface reports on: `today`, `yesterday`, `last_7_days`, `last_14_days`, `last_30_days`, `last_90_days`, `all_time`, or `custom` (pair it with `customRange`). The chart carries the picker, and it moves this — so a host can drive the window and read it back off `periodChanged`. Defaults to `"last_14_days"`.

One of `"today"`, `"yesterday"`, `"last_7_days"`, `"last_14_days"`, `"last_30_days"`, `"last_90_days"`, `"all_time"`, or `"custom"`.

**customRange** `object | null`

The explicit window, as ISO 8601 instants, used when `period` is `custom`. Ignored otherwise. Defaults to `null`.

**properties**

**from** `string` *required*

**to** `string` *required*

**timezone** `"account" | "local"`

Timezone choice for every reported figure. Use `account` for the account's `preferences.ads_scheduling_timezone` or `local` for the viewer's browser timezone. These are the only choices the chart offers. A free-form IANA timezone could make the picker label data incorrectly and would be lost when the viewer changes the selection. To report in another timezone, change the account setting. `account` falls back to the viewer's timezone when the account has no setting. Defaults to `"account"`.

**currency** `string`

Lowercase three-letter ISO 4217 currency code for converted amounts, such as `usd` or `eur`. Unset, it defaults to the account's `preferences.ads_reporting_currency`. Defaults to `""`.

**attributionModel** `"last_touch" | "first_touch"`

Which touch in a buyer’s journey gets the credit: `last_touch`, the default, or `first_touch`. Under both, a Whop touch anywhere in the journey wins the Whop bucket — the model only picks the endpoints. The table carries the picker and moves this, so a host can drive it and read it back off `attributionModelChanged`. Defaults to `"last_touch"`.

**campaignIds** `string[]`

Narrow reporting to these ad campaigns (`adcamp_…`). Empty reports on the whole account. Overridden by `adGroupIds` or `adIds` when either is set. **Pass one ID for an exact figure:** the reporting API scopes by a single source path, so where more than one is given the chart reports on the first alone. Defaults to `[]`.

**adGroupIds** `string[]`

Narrow reporting to these ad groups (`adgrp_…`). Takes precedence over `campaignIds`. Same single-ID caveat. Defaults to `[]`.

**adIds** `string[]`

Narrow reporting to these ads (`ad_…`). The narrowest filter — takes precedence over both others. Same single-ID caveat. Defaults to `[]`.

**tab** `"ads" | "all" | "businesses" | "campaigns" | "ad-groups"`

Which table the `table` element opens on: `campaigns`, `ad-groups`, `ads`, or `all` for the traffic-source view. `all` always reports account-wide — the source series exists only at that level — so a filter set alongside it narrows the table but not the chart. Defaults to `"campaigns"`.

**sources** `string[] | null`

Which traffic the chart reports on, as source paths — `whop:*` for advertising bought through Whop, `ext:*` and `ext:<platform>:*` for ads run elsewhere, `referrer:*` for organic, `direct`, `other`. `null`, the default, reports on Whop advertising alone; `[]` reports on every source; a non-empty array reports on those. A DIFFERENT axis to `campaignIds` and friends, which narrow within Whop advertising — spend-derived ratios stay Whop-attributed whatever this is set to, because spend only ever bought Whop ads. The `table` element’s All tab moves it. Defaults to `null`.

## Events

Pass callbacks in the create options or React props.

### `onEditRequested`

The viewer asked to edit something — a row menu, a draft row, or Edit ad in the details overlay. `campaignId` is always there; `adGroupId` and `adId` narrow it to what they clicked. Only raised when `useCampaignCreator` is off; on, the builder opens over the page instead and this stays quiet.

**payload** `AdsEditTarget`

**properties**

**campaignId** `string` *required*

**adGroupId** `string`

**adId** `string`

## Methods

Call these on the sub handle from `ads.create('reporting', { … })`.

### `update`

Merges new props and callbacks into the sub-controller.

**Parameters**

**options** `Partial\<ReportingSubOptions\>` *required*

### `destroy`

Destroys the sub-controller and its elements, then frees its exclusive slot. A later `create("reporting")` starts fresh.

## Elements

The elements this sub-controller mounts. Each has its own page:

> [card] [ChartElement](https://docs.whop.com/elements/latest/ads/reporting-chart.md)
>
> An account’s performance over a window, with a picker for what to plot — spend, impressions, clicks, or any conversion the account records. Read-only.

> [card] [TableElement](https://docs.whop.com/elements/latest/ads/reporting-table.md)
>
> An advertising account's campaigns, ad groups, and ads in one table, with the tabs that move between them: pick rows to narrow the level below, search, sort, break the numbers down, choose your columns, and pause, resume, duplicate, or delete straight from a row. The window it reports on and the rows it is filtered to live on the handle, so a chart mounted beside it reports on exactly the same thing.


## Related topics

- [ChartElement](https://docs.whop.com/elements/latest/ads/reporting-chart.md)
- [TableElement](https://docs.whop.com/elements/latest/ads/reporting-table.md)
- [Payouts & Money Movement](https://docs.whop.com/developer/payouts/overview.md)
