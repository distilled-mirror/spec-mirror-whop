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

# BreakdownElement

> One currency's total split into available, pending, reserve, and negative balances. Zero-value rows disappear, and the surface deliberately contains no money-movement buttons so the host owns those actions.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Balances`](https://docs.whop.com/elements/latest/wallet/balances), in [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). `accountId` and `accessToken` come from `Wallet`. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

```tsx React theme={null}
import { WhopElements, Wallet, Balances, BreakdownElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <Balances>
          <BreakdownElement onBreakdownSelected={(e) => console.log(e)} />
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
  balances.create('breakdown', { onBreakdownSelected: (e) => console.log(e) }).mount('#wallet-balances-breakdown');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

## Props

**enabled** `boolean`

Off, the element performs no reads and renders a zero balance. Defaults to `true`.

**showSourceCurrency** `boolean`

Show amounts in the selected holding. Turn this off to value the same balance breakdown in USD. Defaults to `true`.

**showTotal** `boolean`

Show the currency title and total above the rows. Turn this off when the surface around it already shows them. Defaults to `true`.

**padded** `boolean`

Draw the surrounding page padding. Turn this off when the surface around it already provides it. Defaults to `true`.

## Events

Pass callbacks in the create options or React props.

### `onBreakdownSelected`

A balance row or its matching bar segment was clicked. The host can open details for `available`, `pending`, `reserve`, or `debt`.

**payload** `object`

**properties**

**category** `"available" | "debt" | "pending" | "reserve"` *required*

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

### `refresh`

Re-fetch the balance after the host moves money.

**Returns** `Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<BreakdownElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-BalanceBreakdownSurface` | A currency balance total, segmented bar, and amount rows |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-BalanceBreakdownSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-BalanceBreakdownSurface': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Balances](https://docs.whop.com/elements/latest/wallet/balances.md)
- [BalanceReportElement](https://docs.whop.com/elements/latest/wallet/reports-balanceReport.md)
- [PaymentDetailElement](https://docs.whop.com/elements/latest/dashboard/paymentDetail.md)
