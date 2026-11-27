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

# CampaignCreatorElement

> The advertising campaign builder: objective and budget, ad groups with their targeting and schedule, and the ads themselves with their creative — the whole flow through to launch. Opens empty to build a new campaign, or on an existing one when you pass `campaignId`. It runs the pixel checker too: a launch is gated on the ad group’s conversion event being live on the destination URL, and the `pixel-setup` wizard opens inside this element to fix an install that is missing. A launch is also gated on the account having signed the Whop Ads agreement: when it has not, the handle opens `ads-agreement` over the builder and the launch carries on once it is signed. Budgets are entered and shown in the account’s `ads_reporting_currency` — the currency its reports already use — and converted to and billed in USD at the current exchange rate. The one token you set on the `ads` handle must carry the pixel scopes alongside the campaign scopes — `ad_campaign:create` and `company:basic:read` — rather than using a separate token per element.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Opens as a modal from [`Ads`](https://docs.whop.com/elements/latest/ads/overview): `ads.createOverlay('campaign-creator')`. Pass props and callbacks in the create options.

> [!NOTE]
>
> This element is **modal-only**. Open it with `createOverlay`; it has no inline mount.

```tsx React theme={null}
import { WhopElements, Ads, CampaignCreatorElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Ads /* options */>
        <CampaignCreatorElement onExited={(e) => console.log(e)} onLaunched={(e) => console.log(e)} />
      </Ads>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const ads = window.WhopElements().ads.create({ /* options */ });
  ads.createOverlay('campaign-creator', {
    onExited: (e) => console.log(e),
    onLaunched: (e) => console.log(e)
  }).open();
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/ads/overview#playground).

## Props

**campaignId** `string`

The campaign (`adcamp_…`) to open. Empty builds a new one. A draft opens where it was left off; a launched campaign opens in edit mode. Defaults to `""`.

**adGroupId** `string`

The ad group (`adgrp_…`) to open in focus, within `campaignId`. Ignored when the campaign has no such group, and when `campaignId` is empty. Defaults to `""`.

**adId** `string`

The ad (`ad_…`) to open in focus, within `adGroupId`. Ignored when that group has no such ad, and when `adGroupId` is empty. Defaults to `""`.

**initialStep** `"ads" | "ad-groups" | "campaign-settings"`

Which step to open on: `campaign-settings`, `ad-groups`, or `ads`. The dashboard deep-links this off its own URL; pass it to drop an account owner straight onto the part they came to change. Read once at mount — moving it later will not walk them between steps. Defaults to `"campaign-settings"`.

**externalMediaIds** `string[]`

File IDs (`file_…`) your app uploaded via `POST /api/v1/files`, offered in the builder's creative library. They show first in the library list, in this order, and while any are set the media pickers open on the library instead of the upload drop zone. Create the files with the same API key that mints the `ads` handle's `accessToken` — the builder reads them back in one batch (`GET /api/v1/files`), and file reads only answer the credential that created them. Pass at most 250 IDs, the batch read's limit. A file appears once its upload has finished processing; an ID that never finishes, or that points at something other than an image or video, is left out. Pass new IDs with `update({ externalMediaIds })` to stream media into a builder that is already open. Defaults to `[]`.

## Events

Pass callbacks in the create options or React props.

### `onExited`

The account owner backed out of the builder without launching. Nothing has been discarded — a draft campaign, if one was started, is already saved. An overlay has already closed itself by the time this fires; react to it if your page needs to.

### `onLaunched`

A campaign went live. `firstLaunch` marks the account’s first ever launched campaign, which is what the dashboard uses to show its welcome state. An overlay has already closed itself; this is where you route onward.

**payload** `object`

**properties**

**campaignId** `string` *required*

**firstLaunch** `boolean` *required*

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

**options** `Partial\<CampaignCreatorElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-PixelSetupSurface` | The pixel setup wizard root — one step at a time |

```ts theme={null}
const ads = whop.ads.create({
  appearance: {
    classes: {
      'whop-PixelSetupSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

ads.update({
  appearance: {
    classes: { 'whop-PixelSetupSurface': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Ads>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Ads](https://docs.whop.com/elements/latest/ads/overview.md)
- [AdsAgreementElement](https://docs.whop.com/elements/latest/ads/ads-agreement.md)
- [Reporting](https://docs.whop.com/elements/latest/ads/reporting.md)
