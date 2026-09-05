> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CardDetailsElement

> One card and what has been spent on it: the card itself, what it has spent against its limit, and its latest transactions. Opens as a drawer. The card face, its reveal and its lock come from the `whopCard` element composed inside, so a host gets one surface rather than assembling three. Pressing a transaction, or asking for the full list, reports the request — the drawer never navigates.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Opens as a modal from [`Wallet`](/elements/beta/wallet/overview): `wallet.createOverlay('cardDetails')`. Pass props and callbacks in the create options.

<Note>This element is **modal-only**. Open it with `createOverlay`; it has no inline mount.</Note>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/cardDetails">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, CardDetailsElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <CardDetailsElement onTransactionSelected={(e) => console.log(e)} onAllTransactionsRequested={(e) => console.log(e)} onTopUpRequested={(e) => console.log(e)} onCloseRequested={(e) => console.log(e)} onMenuRequested={(e) => console.log(e)} />
              </Wallet>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const wallet = window.WhopElements().wallet.create({ /* options */ });
          wallet.createOverlay('cardDetails', {
            onTransactionSelected: (e) => console.log(e),
            onAllTransactionsRequested: (e) => console.log(e),
            onTopUpRequested: (e) => console.log(e),
            onCloseRequested: (e) => console.log(e),
            onMenuRequested: (e) => console.log(e)
          }).open();
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:wallet/cardDetails" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="accessToken" type="string">
  A scoped token for the card and its transactions — needs `payout:account:read`. Omitted, calls carry the viewer's own session, which only answers same-origin.
</ResponseField>

<ResponseField name="card" type="{ status: &#x22;active&#x22; | &#x22;frozen&#x22; | &#x22;canceled&#x22; | &#x22;invited&#x22; | &#x22;denied&#x22; | null; name: string | null; id: string; last4: string | null; billing: Billing | null; expiration_month: string | null; expiration_year: string | null; }">
  Non-sensitive card metadata a host has already loaded. Supplying it saves the card face a list request; its authorized secrets are still fetched so the reveal stays instant.
</ResponseField>

<ResponseField name="cardId" type="string">
  The card this drawer describes, prefixed `icrd_`. Defaults to `""`.
</ResponseField>

<ResponseField name="latestCount" type="number">
  How many recent transactions to list under the card. Defaults to `5`.
</ResponseField>

<ResponseField name="hideTransactions" type="boolean">
  Hide the latest-transactions list, for a host that shows its own beneath the drawer. Defaults to `false`.
</ResponseField>

<ResponseField name="hideMenuButton" type="boolean">
  Hide the overflow button, for a host with no card actions of its own to offer. Defaults to `false`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onTransactionSelected`

A transaction in the drawer was pressed.

**Signature:** `((payload: { transactionId: string; }) => void)`

### `onAllTransactionsRequested`

View all was pressed. Open your own full transaction surface — the drawer never navigates.

**Signature:** `((payload: { cardId: string; }) => void)`

### `onTopUpRequested`

Top up was pressed. Present your own funding surface, such as the wallet's deposit element.

**Signature:** `((payload: { cardId: string; }) => void)`

### `onCloseRequested`

Close was pressed. Dismiss the surface you opened the drawer in — the drawer never closes itself.

**Signature:** `((payload: { cardId: string; }) => void)`

### `onMenuRequested`

The overflow button was pressed. Present your own card actions menu.

**Signature:** `((payload: { cardId: string; }) => void)`

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

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<CardDetailsElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                      | Targets                                  |
| -------------------------- | ---------------------------------------- |
| `.whop-CardDetailsSurface` | The card drawer                          |
| `.whop-CardSurface`        | The card, controls, and revealed details |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-CardDetailsSurface': { borderRadius: '8px', fontWeight: '600' },
      'whop-CardSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-CardDetailsSurface': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.
