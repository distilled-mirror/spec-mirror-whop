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

# WebsitesElement

> An account's websites in one table: every site built on whop.site — listed from the moment it exists, zeros until traffic arrives — merged by hostname with every domain the Whop Pixel reports. Rows for the whop.site ones carry a Whop mark, so the two sources stay apart even when both have traffic. Each row shows a page-view trend, unique visitors, page views, attributed sales, USD revenue, and the last day an event arrived. Expanding a domain loads its most-viewed pages, and each page-view figure opens the breakdown of which events made it up. Set `showVerify` to add a per-row button that asks the page hosting this element to open the site and confirm the pixel is firing; set `showSettings` to add a settings button on whop.site rows that reports the click through `onSettingsRequested`, and `showEdit` to add a primary edit button reporting through `onEditRequested` — your page owns the actual editing.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Websites`](https://docs.whop.com/elements/latest/websites/overview). `accountId` and `windowDays` come from there. Pass props and callbacks through the create options or React props.

```tsx React theme={null}
import { WhopElements, Websites, WebsitesElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Websites /* options */>
        <WebsitesElement onWebsiteToggled={(e) => console.log(e)} onWebsitesLoaded={(e) => console.log(e)} onVerifyRequested={(e) => console.log(e)} onSettingsRequested={(e) => console.log(e)} onEditRequested={(e) => console.log(e)} />
      </Websites>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const websites = window.WhopElements().websites.create({ /* options */ });
  websites.create('websites', {
    onWebsiteToggled: (e) => console.log(e),
    onWebsitesLoaded: (e) => console.log(e),
    onVerifyRequested: (e) => console.log(e),
    onSettingsRequested: (e) => console.log(e),
    onEditRequested: (e) => console.log(e)
  }).mount('#websites-websites');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/websites/overview#playground).

## Props

*This element takes no consumer props.*

## Events

Pass callbacks in the create options or React props.

### `onWebsiteToggled`

A domain row was expanded or collapsed to show or hide its pages.

**payload** `object`

**properties**

**domain** `string` *required*

**expanded** `boolean` *required*

### `onWebsitesLoaded`

The table settled on a list of domains. Fires on every load, not only when the list changes, so a poll that comes back the same still reports — use it to drop any `pendingWebsites` that are now live.

**payload** `object`

**properties**

**domains** `string[]` *required*

### `onVerifyRequested`

The pixel-check button was pressed. Open `url` and watch for pixel events on your side, then push the outcome back through the handle’s `verifyStatus` so the button reflects it.

**payload** `object`

**properties**

**target** `string` *required*

**url** `string` *required*

### `onSettingsRequested`

The settings button on a whop.site row was pressed. The element only reports the click — open your own editing surface for the app.

**payload** `object`

**properties**

**appId** `string` *required*

**route** `string` *required*

**domain** `string` *required*

### `onEditRequested`

The edit button on a whop.site row was pressed. The element only reports the click — open your own editing surface for the app.

**payload** `object`

**properties**

**appId** `string` *required*

**route** `string` *required*

**domain** `string` *required*

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

**options** `Partial\<WebsitesElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-WebsitesTable` | The websites element root — the table of domains |

```ts theme={null}
const websites = whop.websites.create({
  appearance: {
    classes: {
      'whop-WebsitesTable': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

websites.update({
  appearance: { classes: { 'whop-WebsitesTable': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Websites>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Websites](https://docs.whop.com/elements/latest/websites/overview.md)
- [PixelSetupElement](https://docs.whop.com/elements/latest/websites/pixel-setup.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
