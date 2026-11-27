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

# Balances

> Three views of an account's money. The balance view shows the total, a chart of how it changed, and a picker for the time range. The list view shows the holdings that make up that total, valued in dollars. The breakdown view splits one currency into available, pending, reserve, and negative amounts without money-movement controls. When `openHoldingOnSelect` is on, a list row replaces this unit's canvas — the chart and the list — with that holding's balance page instead of only reporting the click.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). Create it to get a handle, then mount its elements on that handle. Call `destroy()` to remove the sub-controller. Create it again to get a fresh handle.

## Preview

A live, interactive demo of this sub-controller's default arrangement with example data:

Mounts anywhere in your view hierarchy. [`BalanceElement`](https://docs.whop.com/elements/latest/wallet/balances-balance#swift) and [`ListElement`](https://docs.whop.com/elements/latest/wallet/balances-list#swift) are independent, so use one alone or both on one screen. Each takes an `accountId` and loads its own data.

## Preview

Running on an iPhone, in the example app:

[BalanceElement running on an iPhone simulator](https://app.revyl.ai/embed/c1954726-8f9b-483d-9f50-247fa7bfb007?controls=0)

```tsx React theme={null}
import { WhopElements, Wallet, Balances, BalanceElement, BreakdownElement, ListElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <Balances>
          <BalanceElement />
          <BreakdownElement />
          <ListElement />
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
  balances.create('balance').mount('#wallet-balances-balance');
  balances.create('breakdown').mount('#wallet-balances-breakdown');
  balances.create('list').mount('#wallet-balances-list');
</script>
```

```swift Swift theme={null}
import SwiftUI
import WhopElements

// WhopSDK.configure(tokenProvider:) runs once at app launch. See Getting started.
struct WalletScreen: View {
    let account = "biz_xxxx"

    var body: some View {
        ScrollView {
            VStack(spacing: 24) {
                BalanceElement(accountId: account)
                ListElement(accountId: account)
            }
            .padding()
        }
    }
}
```

## Options

Pass these to `wallet.create('balances', { … })`, or as props on `<Balances>` in React. Parent-injected props never appear here.

**accessToken** `string`

A scoped token for balance breakdown and settlement reads. An account needs `company:balance:read`. Omitted, the viewer's same-origin session is used.

**openHoldingOnSelect** `boolean`

Show this unit's holding balance page in place of the chart and list when a row is clicked. Off by default so a host that routes on `balanceSelected` is not covered by a second screen. Defaults to `false`.

## Methods

Call these on the sub handle from `wallet.create('balances', { … })`.

### `update`

Merges new props and callbacks into the sub-controller.

**Parameters**

**options** `Partial\<BalancesSubOptions\>` *required*

### `destroy`

Destroys the sub-controller and its elements, then frees its exclusive slot. A later `create("balances")` starts fresh.

## Parameters

**accountId** `String` *required*

The account whose money the view reads. Use an account ID prefixed `biz_`. A user ID prefixed `user_` reads the viewer's own balance and needs viewer authentication (`WhopSDK.configureWithOAuth`), not an account token.

## States

Each view manages its own states, so neither blocks the other from rendering.

## Good to know

* They share no state and load independently, so you can use one alone or both as below.
* Read the detail on [`BalanceElement`](https://docs.whop.com/elements/latest/wallet/balances-balance#swift) and [`ListElement`](https://docs.whop.com/elements/latest/wallet/balances-list#swift); [`ActivityElement`](https://docs.whop.com/elements/latest/wallet/activity#swift) lists the movements behind them.

> [!NOTE]
>
> Call `WhopSDK.configure(tokenProvider:)` once at launch. Views wait for the token. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md). Apply a theme with `.whopTheme(_:)`.

## Elements

The elements this sub-controller mounts. Each has its own page:

> [card] [BalanceElement](https://docs.whop.com/elements/latest/wallet/balances-balance.md)
>
> An account's balance and a chart of how it changed. The total and its change sit at the top, the chart below that, and the time range buttons at the bottom. The viewer can read individual points on the chart and switch the time range themselves.

> [card] [BreakdownElement](https://docs.whop.com/elements/latest/wallet/balances-breakdown.md)
>
> One currency's total split into available, pending, reserve, and negative balances. Zero-value rows disappear, and the surface deliberately contains no money-movement buttons so the host owns those actions.

> [card] [SettlementElement](https://docs.whop.com/elements/latest/wallet/balances-settlement.md)
>
> The account balance details shown by Whop's dashboard: available funds, pending settlement dates, reserve terms, and any negative balance. It opens as a 550px modal and uses the balances handle's account, currency, and credential.

> [card] [ListElement](https://docs.whop.com/elements/latest/wallet/balances-list.md)
>
> The holdings behind an account's balance. Every currency and token gets its own row, showing its name and its value in dollars, largest first. Rows can report which one the viewer tapped, so you can open your own screen for it. When `openHoldingOnSelect` is on, a row click shows that holding's balance page in place of this unit's chart and list.


## Related topics

- [BalanceElement](https://docs.whop.com/elements/latest/wallet/balances-balance.md)
- [BalanceReportElement](https://docs.whop.com/elements/latest/wallet/reports-balanceReport.md)
- [Add Funds to Your Balance](https://docs.whop.com/developer/platforms/add-funds-to-your-balance.md)
