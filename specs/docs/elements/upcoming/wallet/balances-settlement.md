> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# SettlementElement

> The account balance details shown by Whop's dashboard: available funds, pending settlement dates, reserve terms, and any negative balance. It opens as a 550px modal and uses the balances handle's account, currency, and credential.

Opens as a modal from [`Balances`](/elements/upcoming/wallet/balances), in [`Wallet`](/elements/upcoming/wallet/overview): `wallet.create('balances').createOverlay('settlement')`. Pass props and callbacks in the create options.

<Note>This element is **modal-only**. Open it with `createOverlay`; it has no inline mount.</Note>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/balances-settlement">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, Balances, SettlementElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <Balances>
                  <SettlementElement />
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
          balances.createOverlay('settlement').open();
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:balances/settlement" data-whop-elements-version="" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="enabled" type="boolean">
  Off, the Element performs no read and renders an empty details state. Defaults to `true`.
</ResponseField>

<ResponseField name="showSourceCurrency" type="boolean">
  Show amounts in the selected holding. Turn this off to value the details in USD. Defaults to `true`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

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

Re-fetch settlement and reserve details.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<SettlementElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                            | Targets                                        |
| -------------------------------- | ---------------------------------------------- |
| `.whop-BalanceSettlementSurface` | The dashboard-style balance details modal body |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-BalanceSettlementSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-BalanceSettlementSurface': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.
