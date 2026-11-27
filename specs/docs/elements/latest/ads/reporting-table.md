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

# TableElement

> An advertising account's campaigns, ad groups, and ads in one table, with the tabs that move between them: pick rows to narrow the level below, search, sort, break the numbers down, choose your columns, and pause, resume, duplicate, or delete straight from a row. The window it reports on and the rows it is filtered to live on the handle, so a chart mounted beside it reports on exactly the same thing.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Reporting`](https://docs.whop.com/elements/latest/ads/reporting), in [`Ads`](https://docs.whop.com/elements/latest/ads/overview). `accountId` and `accessToken` come from `Ads`. Pass props and callbacks through the create options or React props.

```tsx React theme={null}
import { WhopElements, Ads, Reporting, TableElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Ads /* options */>
        <Reporting>
          <TableElement onTabChanged={(e) => console.log(e)} onFilterChanged={(e) => console.log(e)} onColumnsChanged={(e) => console.log(e)} onCustomMetricsChanged={(e) => console.log(e)} onViewChanged={(e) => console.log(e)} onSourcesChanged={(e) => console.log(e)} onAttributionModelChanged={(e) => console.log(e)} />
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
  reporting.create('table', {
    onTabChanged: (e) => console.log(e),
    onFilterChanged: (e) => console.log(e),
    onColumnsChanged: (e) => console.log(e),
    onCustomMetricsChanged: (e) => console.log(e),
    onViewChanged: (e) => console.log(e),
    onSourcesChanged: (e) => console.log(e),
    onAttributionModelChanged: (e) => console.log(e)
  }).mount('#ads-reporting-table');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/ads/overview#playground).

## Props

**useCampaignCreator** `boolean`

What happens when a viewer asks to edit — from a row menu, a draft row, or Edit ad in the details overlay. On by default: the campaign builder opens over the page, on the campaign and step they asked for, so editing works with nothing wired up. Turn it off to handle campaign editing yourself — nothing opens, and the request comes back on `editRequested`. Defaults to `true`.

**breakdown** `string | null`

No longer has any effect: the table always shows whole rows. Accepted so existing embeds keep working. Defaults to `null`.

**columns** `string[] | null`

The columns on show, by name — standard ones like `impressions` and `roas`, and any pixel events the viewer added. Unset, the table opens on its own defaults. Comes back whole on `columnsChanged`; store the list and pass it here to have the layout stick. Defaults to `null`.

**customMetrics** `CustomMetricDefinition[]`

Columns computed from the figures already in the table — a formula that you name and format. The table can edit them, and every change comes back on `customMetricsChanged` as the whole new list; persist it and pass it here to have it stick. Defaults to `[]`.

**properties**

**name** `string` *required*

**formula** `string` *required*

**format** `"number" | "currency" | "percent"` *required*

**enabled** `boolean` *required*

**sourcesDrill** `string | null`

What the All tab is drilled into, as a source path: `null` for the roots, `ext:*` for a platform list, `ext:<platform>:*` for that platform’s campaigns. Comes back on `viewChanged`. Defaults to `null`.

**query** `string`

What the Ads tab’s search box is filtered to. Defaults to `""`.

**showPausedCampaigns** `boolean`

Whether paused campaigns are listed. Defaults to `true`.

**showPausedAdGroups** `boolean`

Whether paused ad groups are listed. Defaults to `true`.

**showPausedAds** `boolean`

Whether paused ads are listed. Defaults to `true`.

## Events

Pass callbacks in the create options or React props.

### `onTabChanged`

The open tab changed. Mirror it into your own URL if you want it to survive a reload.

**payload** `object`

**properties**

**tab** `"ads" | "all" | "businesses" | "campaigns" | "ad-groups"` *required*

### `onFilterChanged`

The viewer narrowed the view — by ticking rows, or by clicking through a campaign to its ad groups. Every surface under the same handle has already followed it.

**payload** `object`

**properties**

**campaignIds** `string[]` *required*

**adGroupIds** `string[]` *required*

**adIds** `string[]` *required*

### `onColumnsChanged`

The viewer showed or hid a column. Carries every column now on show, standard and pixel-event alike — store the list and pass it straight back as `columns`, or the layout resets on the next mount.

**payload** `object`

**properties**

**columns** `string[]` *required*

### `onCustomMetricsChanged`

The viewer added, edited, or removed a computed column. Carries the entire list either way — store it as given and pass it back as `customMetrics`, or they are gone on the next mount.

**payload** `object`

**properties**

**customMetrics** `CustomMetricDefinition[]` *required*

**properties**

**name** `string` *required*

**formula** `string` *required*

**format** `"number" | "currency" | "percent"` *required*

**enabled** `boolean` *required*

### `onViewChanged`

The viewer moved one of the table’s own controls — the All tab’s drill, the search box, the paused toggle on any of the three tabs. Carries all of them whichever moved; store them and pass them back as props, or they reset on the next mount.

**payload** `object`

**properties**

**sourcesDrill** `string | null` *required*

**breakdown** `null` *required*

**query** `string` *required*

**showPausedCampaigns** `boolean` *required*

**showPausedAdGroups** `boolean` *required*

**showPausedAds** `boolean` *required*

### `onSourcesChanged`

What the All tab is now looking at, as source paths ready to hand to a chart: `null` off the All tab (Whop advertising alone), `[]` for every source, otherwise the ticked rows or the drilled-into group. Every surface under the same handle has already followed it.

**payload** `object`

**properties**

**sources** `string[] | null` *required*

### `onAttributionModelChanged`

The viewer changed which touch gets the credit, from the picker on the All tab. Every surface under the same handle has already re-credited; persist it and pass it back as `attributionModel`, or it reverts to last touch on the next mount.

**payload** `object`

**properties**

**attributionModel** `"last_touch" | "first_touch"` *required*

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

**options** `Partial\<TableElementProps\>` *required*

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.


## Related topics

- [Reporting](https://docs.whop.com/elements/latest/ads/reporting.md)
- [PaymentsTableElement](https://docs.whop.com/elements/latest/dashboard/paymentsTable.md)
- [WebsitesElement](https://docs.whop.com/elements/latest/websites/websites.md)
