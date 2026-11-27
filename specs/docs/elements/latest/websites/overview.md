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

# Websites

> An account's websites: every site built on whop.site plus every domain the Whop Pixel reports, with traffic and attributed revenue per domain. Mount `websites` and it lists them with visitors, page views, sales, revenue, trend, and most-viewed pages. Mount `pixel-setup` alongside it and a site whose pixel the wizard just proved live gets a waiting row automatically, because its first events take a few minutes to reach the stats API and the site would otherwise look lost — you wire nothing up for that, and the row is kept on your page so it is still there if the merchant reloads. Reading stats is privileged, so it needs an `accessToken` — except inside Whop's own app, where the viewer's session carries the read.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

```tsx React theme={null}
import { WhopElements, Websites } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Websites /* options */>
        {/* mount elements here */}
      </Websites>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const websites = window.WhopElements().websites.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.websites.create({ … })`, or as props on `<Websites>` in React.

**windowDays** `number`

How many days of traffic the table covers, from 1 to 90. @default 30 Defaults to `30`.

**includeApps** `boolean`

List websites built on whop.site from the moment they exist, with zeros until traffic arrives. Turn off to show only domains the pixel reports. @default true Defaults to `true`.

**pendingWebsites** `PendingWebsite[]`

Websites you have just added that are not live yet — `{ domain, kind }`, where `kind` is `deploy` for a site on whop.site waiting on its first deploy or `events` for a site waiting on its first pixel events. They render as waiting rows so a site never looks lost right after setup, and the table polls while any is outstanding. Sites the mounted `pixel-setup` wizard proves live are added for you, kept across a reload, and are NOT your job — this is for the waits the handle cannot see for itself, above all a site on whop.site you just created, which no element here reports. Entries you add are yours to remove: a row stops being drawn once its domain reaches the table, but the list itself only changes when you change it, so drop an entry when `websitesLoaded` names its domain and give the rest a timeout of your own. Defaults to `[]`.

**properties**

**domain** `string` *required*

**kind** `"events" | "deploy"` *required*

**showVerify** `boolean`

Add a pixel-check button to each row. The element only reports the press through `onVerifyRequested` — opening the site and deciding the outcome is yours, because a popup opened by your own page is the one browsers trust. Defaults to `false`.

**showSettings** `boolean`

Add a settings button to whop.site rows. The element only reports the press through `onSettingsRequested` — the editing surface is yours. Defaults to `false`.

**showEdit** `boolean`

Add an edit button to whop.site rows, styled as the row’s primary action. The element only reports the press through `onEditRequested` — the editing surface is yours. Defaults to `false`.

**verifyTarget** `string`

Which row the check in progress belongs to — the `target` from the `verifyRequested` event you are handling. Defaults to `""`.

**verifyStatus** `string`

How the check in progress is going, shown on that row’s button: `waiting`, `connected`, `not-detected`, or `blocked` when the popup was blocked. Defaults to `"idle"`.

One of `"idle"`, `"waiting"`, `"connected"`, `"not-detected"`, or `"blocked"`.

**accessToken** `string`

A scoped token for the reads — stats need `stats:read`, and the whop.site website list reads apps (rows degrade to traffic-only if the token can’t). Mint it on your server with `POST /api/v1/access_tokens` and set a fresh one before it expires. `pixel-setup` is the one child this is not projected into — it authenticates its own scans from its own `accessToken` — so mint a single token carrying both sets of scopes and pass that same string to both, rather than one token per element. Omitted, the reads carry the viewer’s own session, which only answers same-origin.

**accountId** `string` *required*

Account ID, prefixed `biz_`, whose websites these surfaces read.

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

Call these on the Websites handle from `whop.websites.create({ … })` or `useWebsites()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<WebsitesOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Elements

The elements this group mounts. Each has its own page:

> [card] [WebsitesElement](https://docs.whop.com/elements/latest/websites/websites.md)
>
> An account's websites in one table: every site built on whop.site — listed from the moment it exists, zeros until traffic arrives — merged by hostname with every domain the Whop Pixel reports. Rows for the whop.site ones carry a Whop mark, so the two sources stay apart even when both have traffic. Each row shows a page-view trend, unique visitors, page views, attributed sales, USD revenue, and the last day an event arrived. Expanding a domain loads its most-viewed pages, and each page-view figure opens the breakdown of which events made it up. Set `showVerify` to add a per-row button that asks the page hosting this element to open the site and confirm the pixel is firing; set `showSettings` to add a settings button on whop.site rows that reports the click through `onSettingsRequested`, and `showEdit` to add a primary edit button reporting through `onEditRequested` — your page owns the actual editing.

> [card] [PixelSetupElement](https://docs.whop.com/elements/latest/websites/pixel-setup.md)
>
> Installs the Whop Pixel and wires conversion events: copy the snippet, check a page for it, confirm the events fire. Mount it inline, or inside your own overlay.


## Related topics

- [WebsitesElement](https://docs.whop.com/elements/latest/websites/websites.md)
- [Website Hosting on Whop](https://docs.whop.com/developer/websites/hosting.md)
- [Host a Website on Whop](https://docs.whop.com/developer/websites/overview.md)
