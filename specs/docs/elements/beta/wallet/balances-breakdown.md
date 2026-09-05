> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# BreakdownElement

> One currency's total split into available, pending, reserve, and negative balances. Zero-value rows disappear, and the surface deliberately contains no money-movement buttons so the host owns those actions.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Balances`](/elements/beta/wallet/balances), in [`Wallet`](/elements/beta/wallet/overview). `accountId` and `accessToken` come from `Wallet`. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/balances-breakdown">
      <CodeGroup>
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
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const wallet = window.WhopElements().wallet.create({ /* options */ });
          const balances = wallet.create('balances', { /* options */ });
          balances.create('breakdown', { onBreakdownSelected: (e) => console.log(e) }).mount('#wallet-balances-breakdown');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:balances/breakdown" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="enabled" type="boolean">
  Off, the element performs no reads and renders a zero balance. Defaults to `true`.
</ResponseField>

<ResponseField name="showSourceCurrency" type="boolean">
  Show amounts in the selected holding. Turn this off to value the same balance breakdown in USD. Defaults to `true`.
</ResponseField>

<ResponseField name="showTotal" type="boolean">
  Show the currency title and total above the rows. Turn this off when the surface around it already shows them. Defaults to `true`.
</ResponseField>

<ResponseField name="padded" type="boolean">
  Draw the surrounding page padding. Turn this off when the surface around it already provides it. Defaults to `true`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onBreakdownSelected`

A balance row or its matching bar segment was clicked. The host can open details for `available`, `pending`, `reserve`, or `debt`.

**Signature:** `((payload: { category: "pending" | "available" | "debt" | "reserve"; }) => void)`

### `onLoaderStart`

Runs after the loading skeleton first paints and before `onReady`.

**Signature:** `(() => void)`

### `onReady`

Runs after the element's first complete paint.

**Signature:** `(() => void)`

### `onError`

Runs when the element fails to load or crashes. The fallback remains visible. Use `code` for programmatic handling. `sourceKey` identifies a failed host-state source.

**Signature:** `((e: { message: string; code?: string | undefined; sourceKey?: string | undefined; }) => void)`

## Methods

Call these on the handle returned by `create`, or through a React `ref`.

### `refresh`

Re-fetch the balance after the host moves money.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<BreakdownElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                           | Targets                                                  |
| ------------------------------- | -------------------------------------------------------- |
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
