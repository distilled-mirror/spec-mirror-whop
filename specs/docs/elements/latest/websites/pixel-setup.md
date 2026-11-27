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

# PixelSetupElement

> Installs the Whop Pixel and wires conversion events: copy the snippet, check a page for it, confirm the events fire. Mount it inline, or inside your own overlay.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Websites`](https://docs.whop.com/elements/latest/websites/overview). Pass props and callbacks through the create options or React props.

> [!NOTE]
>
> You can mount this element **inline** (`create`) or open it as a **modal** overlay (`createOverlay`).

```tsx React theme={null}
import { WhopElements, Websites, PixelSetupElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Websites /* options */>
        <PixelSetupElement onRedirectUrlChanged={(e) => console.log(e)} onScanned={(e) => console.log(e)} onFinished={(e) => console.log(e)} onUrlSubmitted={(e) => console.log(e)} onVerifyLaunched={(e) => console.log(e)} onVerifyConnected={(e) => console.log(e)} onVerdict={(e) => console.log(e)} onEventVerified={(e) => console.log(e)} />
      </Websites>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const websites = window.WhopElements().websites.create({ /* options */ });
  websites.create('pixel-setup', {
    onRedirectUrlChanged: (e) => console.log(e),
    onScanned: (e) => console.log(e),
    onFinished: (e) => console.log(e),
    onUrlSubmitted: (e) => console.log(e),
    onVerifyLaunched: (e) => console.log(e),
    onVerifyConnected: (e) => console.log(e),
    onVerdict: (e) => console.log(e),
    onEventVerified: (e) => console.log(e)
  }).mount('#websites-pixel-setup');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/websites/overview#playground).

## Props

**accountId** `string`

Account ID, prefixed `biz_`, whose pixel this is. Required; the snippet, scans, and invites all run against it. Defaults to `""`.

**accessToken** `string`

A scoped token for the scans and the developer invite. The scans need `company:basic:read`; “Invite a developer” additionally needs `authorized_user:create`, and the event presets read the account with `company:balance:read` (without it the wizard still works, on a generic preset list). When no `destinationUrl` is passed, the empty URL field also suggests pages from the account’s ads and pixel traffic — those reads need `ad_campaign:basic:read` and `stats:read`, and without them the field is simply bare. Mint it on your server with `POST /api/v1/access_tokens` and set a fresh one before it expires. Another element that opens this wizard inside itself — the ads `campaign-creator` does — runs these calls on its own token, so add these scopes to that one token rather than minting a second: this element has no way to read a credential you set elsewhere. Omitted, the calls carry the viewer’s own session, which only answers same-origin.

**destinationUrl** `string`

The page to check, scanned on mount. Leave it blank and the wizard asks for one. Also what gates "Verify live" on the events step — that button walks the account owner through their own funnel to catch events a server-side scan cannot see, so it only appears once a destination is known, from here or named in the wizard. Read once at boot, so `update()` will not retarget mid-flow — mint a fresh mount for a different page. Defaults to `""`.

**eventName** `string`

Optional. An event that must fire on that page (`lead`, `schedule`, `view_content`, …); the events step blocks until it is detected. Unset, every event is offered and none is required. Events Whop records server-side (`purchase`, `subscribe`, trials, checkout steps) are ignored. Read once at boot. Defaults to `""`.

**redirectUrl** `string`

A URL the destination redirects to, when the event fires there instead. The account owner can also set one in the wizard — listen for `redirectUrlChanged` and pass it back next time. Defaults to `""`.

**initialStep** `"events" | "install"`

Where the wizard opens: the install step, or straight onto `events` when the pixel is already there and only the conversion event is missing. Defaults to `"install"`.

**showIntro** `boolean`

Open on the "why a pixel" splash. Turn it off where the account owner already knows why they are here — a campaign flow that sends them in mid-launch — and the wizard opens straight on the install step. Defaults to `true`.

**showInviteDeveloper** `boolean`

Offer the "Invite a developer to install it" path. Hide it where the people using your embed should not be inviting members onto the Whop team behind it. Defaults to `true`.

**showOptimizePrompt** `boolean`

Show the "your pixel is live" screen after a page checks out, which offers conversion events as the next thing to do. Turn it off where the account owner came for the events themselves — a passing check then goes straight to them instead of pausing on a screen they would click through. Defaults to `true`.

**verifyFirst** `boolean`

Lead with the page check instead of the install instructions: the URL field (and its suggestions) sits at the top, the install paths fold under a "Pixel not installed yet?" toggle, and the header reads "Verify your pixel". For an account owner who already has a pixel and is here to find out why nothing is being attributed. Pair it with `showIntro: false`. Defaults to `false`.

## Events

Pass callbacks in the create options or React props.

### `onRedirectUrlChanged`

The account owner set or cleared a redirect URL in the wizard. Persist it and pass it back as `redirectUrl` next time.

**payload** `object`

**properties**

**url** `string` *required*

### `onScanned`

A pixel scan settled. Fires for every URL the wizard checks, so your own install badge can follow it. `primary` marks the page being set up: the funnel checker scans whatever else the account owner lists, other hostnames included, so gate on it before you treat a result as being about their site. You do not need this to keep a `websites` table honest — mounted under that handle, a site proven live already gets its waiting row.

**payload** `object`

**properties**

**url** `string` *required*

**status** `"installed" | "missing" | "unreachable"` *required*

**installed** `boolean` *required*

**primary** `boolean` *required*

### `onFinished`

Done was pressed. Re-check the page yourself — this says the wizard finished, not that your requirements are met.

### `onUrlSubmitted`

The account owner named the page to check from inside the wizard — typed by hand, or picked from the suggested pages. Only reachable when no destination was supplied at mount.

**payload** `object`

**properties**

**url** `string` *required*

**source** `"typed" | "suggested"` *required*

### `onVerifyLaunched`

"Verify live" was pressed. `popupBlocked` means the browser refused the funnel window, so the walk never started.

**payload** `object`

**properties**

**url** `string` *required*

**popupBlocked** `boolean` *required*

### `onVerifyConnected`

The funnel page pinged the wizard back — the pixel is loaded there and the walk is live.

**payload** `object`

**properties**

**url** `string` *required*

### `onVerdict`

The verify-live walk finished. `ok` means no tracking gaps; otherwise `gaps` carries the gap codes the walk found.

**payload** `object`

**properties**

**url** `string` *required*

**ok** `boolean` *required*

**gaps** `string[]` *required*

**eventsCaptured** `number` *required*

### `onEventVerified`

The account owner walked their funnel with "Verify live" and this event fired — proof it is wired where a server-side scan cannot reach.

**payload** `object`

**properties**

**url** `string` *required*

**event** `string` *required*

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

**options** `Partial\<PixelSetupElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-PixelSetupSurface` | The pixel setup wizard root — one step at a time |

```ts theme={null}
const websites = whop.websites.create({
  appearance: {
    classes: {
      'whop-PixelSetupSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

websites.update({
  appearance: {
    classes: { 'whop-PixelSetupSurface': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Websites>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Websites](https://docs.whop.com/elements/latest/websites/overview.md)
- [CampaignCreatorElement](https://docs.whop.com/elements/latest/ads/campaign-creator.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
