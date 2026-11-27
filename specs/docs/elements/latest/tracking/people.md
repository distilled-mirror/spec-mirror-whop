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

# PeopleElement

> Everyone the account has seen — visitors and customers resolved from pixel activity, with their source, spend, and activity counters. Search, filter by source, event, geography, or device, sort any column, and page through. A row click follows `links.person` when the handle configures it, and otherwise comes back to you as `personOpened`.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Tracking`](https://docs.whop.com/elements/latest/tracking/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props.

```tsx React theme={null}
import { WhopElements, Tracking, PeopleElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Tracking /* options */>
        <PeopleElement onPeriodChanged={(e) => console.log(e)} onAttributionModelChanged={(e) => console.log(e)} onFiltersChanged={(e) => console.log(e)} onSortChanged={(e) => console.log(e)} onPersonOpened={(e) => console.log(e)} />
      </Tracking>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const tracking = window.WhopElements().tracking.create({ /* options */ });
  tracking.create('people', {
    onPeriodChanged: (e) => console.log(e),
    onAttributionModelChanged: (e) => console.log(e),
    onFiltersChanged: (e) => console.log(e),
    onSortChanged: (e) => console.log(e),
    onPersonOpened: (e) => console.log(e)
  }).mount('#tracking-people');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/tracking/overview#playground).

## Props

**source** `string`

Narrow to people acquired by a traffic source, as a source path — `whop:adcamp_…:*` for one of the account’s campaigns, `ext:*` for ads run elsewhere, `referrer:*`, `direct`. The same value the ads table’s click counts link with, so a page can hydrate this straight from its own URL. Defaults to `""`.

**eventName** `string`

Narrow to people who performed an event, by its name — `payment.completed`, `pixel.lead`, `pixel.page`. Defaults to `""`.

**customEvent** `string`

Narrow to people who performed a custom pixel event, by its custom name. Pairs with an empty `eventName`. Defaults to `""`.

**utmSource** `string`

Narrow by the `utm_source` their traffic carried. Comma-separate to match any of several. Defaults to `""`.

**country** `string`

Narrow by ISO 3166-1 alpha-2 country code, such as `US` or `GB`. Defaults to `""`.

**device** `string`

Narrow by device class — `desktop`, `mobile`, `tablet`, `bot`. Defaults to `""`.

**browser** `string`

Narrow by browser name. Defaults to `""`.

**os** `string`

Narrow by operating system name. Defaults to `""`.

**hasPurchased** `string`

`true` for customers only, `false` for people who never bought, empty for everyone. Defaults to `""`.

**query** `string`

What the search box is filtered to — a name, an email, or an identifier. Defaults to `""`.

**audienceId** `string`

Narrow to a saved audience (`adaud_…`) built from people filters. The audience filter menu moves this. Defaults to `""`.

**eventFrom** `string`

Pin the event window for `source`/`eventName` filters to an exact ISO 8601 instant instead of the handle’s `period`. Set both `eventFrom` and `eventTo` or neither. Defaults to `""`.

**eventTo** `string`

The pinned event window’s end, as an ISO 8601 instant. Defaults to `""`.

**sort** `string`

The sorted column: `name`, `email`, `ltv`, `purchase_count`, `event_count`, `first_seen_at`, `last_seen_at`. Defaults to `"last_seen_at"`.

**dir** `"asc" | "desc"`

The sort direction. Defaults to `"desc"`.

## Events

Pass callbacks in the create options or React props.

### `onPeriodChanged`

The viewer moved the window from the period selector. Every surface under the same handle has already followed it; persist it and pass it back as `period`/`customRange` on the handle, or the window resets on the next mount.

**payload** `object`

**properties**

**period** `string` *required*

One of `"today"`, `"yesterday"`, `"last_7_days"`, `"last_14_days"`, `"last_30_days"`, `"last_90_days"`, `"all_time"`, or `"custom"`.

**customRange** `object | null` *required*

**properties**

**from** `string` *required*

**to** `string` *required*

### `onAttributionModelChanged`

The viewer changed which touch gets the credit, from the filter row’s Attribution menu. Every surface under the same handle has already re-credited; persist it and pass it back as `attributionModel` on the handle, or it reverts on the next mount.

**payload** `object`

**properties**

**attributionModel** `"last_touch" | "first_touch"` *required*

### `onFiltersChanged`

The viewer moved a filter, the search box, or the audience picker. Carries the entire filter state whichever moved — mirror it into your own URL and pass it back as props to make the view reproducible.

**payload** `PeopleFilterBag`

**properties**

**source** `string` *required*

**eventName** `string` *required*

**customEvent** `string` *required*

**utmSource** `string` *required*

**country** `string` *required*

**device** `string` *required*

**browser** `string` *required*

**os** `string` *required*

**hasPurchased** `string` *required*

**query** `string` *required*

**audienceId** `string` *required*

**eventFrom** `string` *required*

**eventTo** `string` *required*

### `onSortChanged`

The viewer sorted a column. Persist and pass back as `sort`/`dir`, or sorting resets on the next mount.

**payload** `object`

**properties**

**sort** `string` *required*

**dir** `"asc" | "desc"` *required*

### `onPersonOpened`

The viewer clicked a person row and the handle has no `links.person` to follow. Present the `person` element with it, e.g. `createOverlay("person", { identifier: payload.personId })` (`personId` is the stable per-person key; `identifier` is the most human-meaningful one the surface has), or pass `links.person` on the handle to turn rows into real anchors instead.

**payload** `object`

**properties**

**identifier** `string` *required*

**personId** `string` *required*

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

**options** `Partial\<PeopleElementProps\>` *required*

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.


## Related topics

- [Tracking](https://docs.whop.com/elements/latest/tracking/overview.md)
- [PersonElement](https://docs.whop.com/elements/latest/tracking/person.md)
- [What You Can Build with Whop](https://docs.whop.com/developer/start.md)
