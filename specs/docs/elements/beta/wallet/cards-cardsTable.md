> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CardsTableElement

> A sortable table of every issued card on an account, including cardholder, last month's spend, limit, and creation date. Rows and the create button emit events so the host owns card details and issuance flows.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Cards`](/elements/beta/wallet/cards), in [`Wallet`](/elements/beta/wallet/overview). `accountId` and `accessToken` come from `Wallet`. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/cards-cardsTable">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, Cards, CardsTableElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <Cards>
                  <CardsTableElement onCardSelected={(e) => console.log(e)} onCreateCardRequested={(e) => console.log(e)} />
                </Cards>
              </Wallet>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const wallet = window.WhopElements().wallet.create({ /* options */ });
          const cards = wallet.create('cards', { /* options */ });
          cards.create('cardsTable', {
            onCardSelected: (e) => console.log(e),
            onCreateCardRequested: (e) => console.log(e)
          }).mount('#wallet-cards-cardsTable');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:cards/cardsTable" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="accessToken" type="string">
  A scoped token for the privileged reads. Card rows need `payout:account:read`; cardholder names also need `company:authorized_user:read`. Omitted, calls carry the viewer's own session, which only answers same-origin.
</ResponseField>

<ResponseField name="enabled" type="boolean">
  Off, the element does not fetch and renders its shell with no card rows. Defaults to `true`.
</ResponseField>

<ResponseField name="hideCreateButton" type="boolean">
  Hide the create virtual card button. Off by default. Defaults to `false`.
</ResponseField>

<ResponseField name="showFinancials" type="boolean">
  Show last month's spend. Turn this off when the viewer may see cards but not account financials. Defaults to `true`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onCardSelected`

A row was clicked. Open the host-owned card details experience for this card.

**Signature:** `((payload: { cardId: string; }) => void)`

### `onCreateCardRequested`

The create virtual card button was clicked.

**Signature:** `((payload: Record<string, never>) => void)`

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

Re-fetch the cards and cardholders after the host creates, freezes, cancels, or renames a card.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<CardsTableElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                     | Targets                         |
| ------------------------- | ------------------------------- |
| `.whop-CardsTableRow`     | One issued card row             |
| `.whop-CardsTableSurface` | The complete issued-cards table |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-CardsTableRow': { borderRadius: '8px', fontWeight: '600' },
      'whop-CardsTableSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: { classes: { 'whop-CardsTableRow': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.
