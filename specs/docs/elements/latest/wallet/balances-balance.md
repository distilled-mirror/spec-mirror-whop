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

# BalanceElement

> An account's balance and a chart of how it changed. The total and its change sit at the top, the chart below that, and the time range buttons at the bottom. The viewer can read individual points on the chart and switch the time range themselves.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Balances`](https://docs.whop.com/elements/latest/wallet/balances), in [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). `accountId` and `currency` come from `Wallet`. Pass props and callbacks through the create options or React props.

Mounts anywhere in your view hierarchy. Takes an `accountId` and loads its own data; `range` picks the time range it opens on. The root is a `VStack`, so it sizes to its content and fills the width you give it.

```tsx React theme={null}
import { WhopElements, Wallet, Balances, BalanceElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <Balances>
          <BalanceElement onRangeChanged={(e) => console.log(e)} onRangeBrushed={(e) => console.log(e)} />
        </Balances>
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  const balances = wallet.create('balances', { /* options */ });
  balances.create('balance', {
    onRangeChanged: (e) => console.log(e),
    onRangeBrushed: (e) => console.log(e)
  }).mount('#wallet-balances-balance');
</script>
```

```swift Swift theme={null}
import SwiftUI
import WhopElements

// WhopSDK.configure(tokenProvider:) runs once at app launch. See Getting started.
struct WalletScreen: View {
    var body: some View {
        BalanceElement(
            accountId: "biz_xxxx",
            range: .oneMonth
        )
    }
}
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

[BalanceElement running on an iPhone simulator](https://app.revyl.ai/embed/bb0f1c0a-4242-486a-a5f5-03dd9711744c?controls=0)

## Props

**accessToken** `string`

A scoped token for the reads. An account needs `company:balance:read` for the total and `stats:read` for the chart; a user's own balance needs `user:balance:read`. Mint it on your server with `POST /api/v1/access_tokens`, and set a fresh one before it expires. Without it the reads use the viewer's own session, which only works same-origin. Defaults to `""`.

**range** `"1D" | "1W" | "1M" | "1Y" | "ALL"`

The time range to open on. The buttons change it from there, or set this prop again to control it yourself. Defaults to `"1M"`.

**openedAt** `string`

When the account was opened, as an ISO 8601 timestamp. `ALL` starts here instead of at its five-year default, so an account older than that keeps its full history. Defaults to `""`.

**accountName** `string`

Adds the account name to the label: `Total balance · Acme` normally, and `Acme balance on Jul 9` while the viewer reads a point. Without it the label is just `Total balance`. Defaults to `""`.

**brush** `object | null`

The selected range on the chart, as ISO 8601 timestamps. Set it to control the selection yourself, or `null` to clear the one the viewer made. Echo back what `rangeBrushed` reports, and clearing a filter elsewhere on your page clears the selection here too. Defaults to `null`.

**properties**

**from** `string` *required*

**to** `string` *required*

**markedAt** `string`

A moment to mark on the chart, as an ISO 8601 timestamp — the point something else on your page is pointing at, like the row under the pointer in an activity list. It reads like the viewer hovering that point themselves, and is ignored when it falls outside the window on screen or when they are reading the chart. Defaults to `""`.

**includeOwnedAccounts** `boolean`

For a user, total their own money plus every account they own instead of their own money alone. Ignored for an account, which is a single account already. Defaults to `false`.

**hideTotal** `boolean`

Hide the total and its change, leaving the chart and the range buttons. Defaults to `false`.

**hideChart** `boolean`

Hide the chart, leaving the total and the range buttons. Defaults to `false`.

**height** `number`

How tall the chart is drawn, in pixels. Defaults to `210`.

## Events

Pass callbacks in the create options or React props.

### `onRangeChanged`

The visible time range changed. Mirror it if something else on your page reads the same period.

**payload** `object`

**properties**

**range** `"1D" | "1W" | "1M" | "1Y" | "ALL"` *required*

### `onRangeBrushed`

The viewer dragged across the chart to select a range, or cleared the selection (`null`). Gives ISO 8601 timestamps plus the range they were read in, so you can filter your own views, like an activity list or a table, to the same period.

**payload** `object | null`

**properties**

**from** `string` *required*

**to** `string` *required*

**range** `"1D" | "1W" | "1M" | "1Y" | "ALL"` *required*

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

**options** `Partial\<BalanceElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-Balance` | The total, its line, and the window picker |
| `.whop-BalanceBrushSummary` | The measured stretch |
| `.whop-BalanceChart` | The balance line |
| `.whop-BalanceRanges` | The window picker |
| `.whop-BalanceTotal` | The total and its move |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-Balance': { borderRadius: '8px', fontWeight: '600' },
      'whop-BalanceBrushSummary': { borderRadius: '8px', fontWeight: '600' },
      'whop-BalanceChart': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

// 5 classes use this shape
wallet.update({
  appearance: { classes: { 'whop-Balance': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.

## Parameters

**accountId** `String` *required*

The account whose money the view reads. Use an account ID prefixed `biz_`. A user ID prefixed `user_` reads the viewer's own balance and needs viewer authentication (`WhopSDK.configureWithOAuth`), not an account token.

**currency** `String`

The currency the total and chart are shown in. Defaults to `"usd"`.

**range** `WalletBalanceRange`

The time range the chart opens on: `.oneDay`, `.oneWeek`, `.oneMonth`, `.oneYear`, `.all`. Defaults to `.oneMonth`, and the viewer can change it with the picker.

## States

Shows a placeholder chart while the data loads. If the comparison period fails, the change is hidden rather than guessed, so any number on screen is one that was actually measured. An account ID that can't be resolved shows `Couldn't load this account.` instead of spinning.

## Good to know

* Press and drag across the chart to scrub it. The headline follows the point under your finger, and returns to the total when you let go.
* Balance history is available only for account IDs prefixed `biz_`, so a personal account prefixed `user_` renders the total without a chart.
* Pairs with [`ListElement`](https://docs.whop.com/elements/latest/wallet/balances-list#swift), which breaks the same total into individual holdings.

> [!NOTE]
>
> Call `WhopSDK.configure(tokenProvider:)` once at launch. Views wait for the token. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md). Apply a theme with `.whopTheme(_:)`.


## Related topics

- [Balances](https://docs.whop.com/elements/latest/wallet/balances.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
- [ListElement](https://docs.whop.com/elements/latest/wallet/balances-list.md)
