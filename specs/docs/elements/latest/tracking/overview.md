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

# Tracking

> An account's tracked audience: everyone its pixel has seen, and every event they performed. Scope it to an account with `accountId`, then mount `people`, `events`, or both. They read the same window and the same credit rule, so two surfaces side by side can never disagree about what they are showing. `person` is where a click on either of them leads: one person's whole record, mountable inline or presentable over your page. `links` wires their person links to your own pages.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

```tsx React theme={null}
import { WhopElements, Tracking } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Tracking /* options */>
        {/* mount elements here */}
      </Tracking>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const tracking = window.WhopElements().tracking.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.tracking.create({ … })`, or as props on `<Tracking>` in React.

**accountId** `string`

Account ID, prefixed `biz_`, whose people and events this reads. Required. Defaults to `""`.

**accessToken** `string`

A scoped token both surfaces read with. Mint one token for the whole handle on your server with `POST /api/v1/access_tokens`, and set a fresh one with `update({ accessToken })` before it expires. Reading the lists needs `member:basic:read` or `company:basic:read`; the filter menus' option counts additionally use `stats:read`, and the People table's saved-audience filter uses `audience:basic:read` — without those the menus simply show fewer options. Omitted, the calls carry the viewer's own session, which only answers same-origin.

**links** `CrossLinksConfig`

Where person links lead on your site. `person` is an absolute-URL template with two placeholders, each replaced URL-encoded per row: `{personId}` is the stable `prsn_…` ID, identical for the same person on every surface, so key your page on it and mount the `person` element there; `{identifier}` is the most human-meaningful identifier the surface has (a user ID or email where known), so it can differ across surfaces. It must be absolute (`https://…`); a relative URL would resolve against the element frame, so one is treated as unset. Unset, a person click raises `personOpened` on its element instead, which is the cue to present `person` over your page. `people` and `events` base URLs are accepted for symmetry with the ads handle but nothing under this handle renders them. Defaults to `{}`.

**properties**

**people** `string`

**events** `string`

**person** `string`

**period** `string`

The window both surfaces cover: `today`, `yesterday`, `last_7_days`, `last_14_days`, `last_30_days`, `last_90_days`, `all_time`, or `custom` (pair it with `customRange`). The elements carry the picker and move this; read it back off `periodChanged`. Defaults to `"last_14_days"`.

One of `"today"`, `"yesterday"`, `"last_7_days"`, `"last_14_days"`, `"last_30_days"`, `"last_90_days"`, `"all_time"`, or `"custom"`.

**customRange** `object | null`

The explicit window, as ISO 8601 instants, used when `period` is `custom`. Ignored otherwise. Defaults to `null`.

**properties**

**from** `string` *required*

**to** `string` *required*

**timezone** `"account" | "local"`

Which of two zones the window resolves in: `account`, the account's own scheduling zone, or `local`, whichever zone the viewer's browser is in. `account` falls back to the viewer's own when the account has set none. Defaults to `"account"`.

**attributionModel** `"last_touch" | "first_touch"`

Which touch in a person’s journey gets the credit when filtering by source: `last_touch`, the default, or `first_touch`. The elements carry the picker and move this, so a host can drive it and read it back off `attributionModelChanged`. Defaults to `"last_touch"`.

**appearance** `Appearance`

Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.

**locale** `WhopElementsLocale`

Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.

## Events

Pass callbacks in the create options or React props.

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**loading** `boolean`

## Methods

Call these on the Tracking handle from `whop.tracking.create({ … })` or `useTracking()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<TrackingOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Elements

The elements this group mounts. Each has its own page:

> [card] [PeopleElement](https://docs.whop.com/elements/latest/tracking/people.md)
>
> Everyone the account has seen — visitors and customers resolved from pixel activity, with their source, spend, and activity counters. Search, filter by source, event, geography, or device, sort any column, and page through. A row click follows `links.person` when the handle configures it, and otherwise comes back to you as `personOpened`.

> [card] [EventsElement](https://docs.whop.com/elements/latest/tracking/events.md)
>
> Every event the account measured — page views, leads, purchases, and custom pixel events — as a raw, filterable stream. The rows behind any metric: filter by event, source, geography, or device over the handle’s window, or search one person’s identifier to read their whole journey. A person cell follows `links.person` when the handle configures it, and otherwise comes back to you as `personOpened`.

> [card] [PersonElement](https://docs.whop.com/elements/latest/tracking/person.md)
>
> Everything the account knows about one person: who they are, where they came from, what they have spent, and every event they have performed, in order. The view a person click leads to: mount it on the page `links.person` points at, or present it over your own page when `people` or `events` raise `personOpened`. `identifier` takes any identifier a person has been seen under, so a `personOpened` payload resolves here unchanged.


## Related topics

- [TrackingElement](https://docs.whop.com/elements/latest/delivery/tracking.md)
- [Retrieve Account Customer Tracking](https://docs.whop.com/api-reference/accounts/retrieve-account-customer-tracking.md)
- [Affiliates](https://docs.whop.com/developer/guides/affiliates.md)
