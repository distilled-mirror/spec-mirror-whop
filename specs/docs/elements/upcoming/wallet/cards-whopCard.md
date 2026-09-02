> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# WhopCardElement

> Renders one issued Whop Card. The element prefetches authorized secrets immediately after it mounts with a card ID, but keeps them masked until the viewer clicks it or selects `View details`. The API remains the permission authority. Includes lock and unlock controls by default.

Mounts inside [`Cards`](/elements/upcoming/wallet/cards), in [`Wallet`](/elements/upcoming/wallet/overview). `accountId` and `accessToken` come from `Wallet`. Pass props and callbacks through the create options or React props.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/cards-whopCard">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, Cards, WhopCardElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <Cards>
                  <WhopCardElement onDetailsChanged={(e) => console.log(e)} onLockRequested={(e) => console.log(e)} onLockChanged={(e) => console.log(e)} onAppleWalletRequested={(e) => console.log(e)} />
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
          cards.create('whopCard', {
            onDetailsChanged: (e) => console.log(e),
            onLockRequested: (e) => console.log(e),
            onLockChanged: (e) => console.log(e),
            onAppleWalletRequested: (e) => console.log(e)
          }).mount('#wallet-cards-whopCard');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:cards/whopCard" data-whop-elements-version="" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="accessToken" type="string">
  A scoped token for card access. Omitted calls carry the viewer session when same-origin. Secrets are returned only when the API authorizes the viewer.
</ResponseField>

<ResponseField name="cardId" type="string">
  Issued card ID, prefixed `icrd_`. Defaults to `""`.
</ResponseField>

<ResponseField name="hideControls" type="boolean">
  Hide the View details and Lock card buttons. The card itself remains clickable to reveal details. Defaults to `false`.
</ResponseField>

<ResponseField name="disableLock" type="boolean">
  Disable the lock control while keeping it visible. Useful when a host has temporarily frozen card actions. Defaults to `false`.
</ResponseField>

<ResponseField name="hideAppleWalletButton" type="boolean">
  Hide the Add to Apple Wallet button. It is shown for active and locked cards by default. Defaults to `false`.
</ResponseField>

<ResponseField name="deferLockToHost" type="boolean">
  Direct-mode integration option. Emits `lockRequested` without updating the card so a first-party host can retain its confirmation flow. Defaults to `false`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onDetailsChanged`

Fires after details are revealed or hidden.

**Signature:** `((payload: { cardId: string; revealed: boolean; }) => void)`

### `onLockRequested`

Fires instead of updating when `deferLockToHost` is true.

**Signature:** `((payload: { cardId: string; locked: boolean; }) => void)`

### `onLockChanged`

Fires after the Cards REST API successfully locks or unlocks the card.

**Signature:** `((payload: { cardId: string; locked: boolean; }) => void)`

### `onAppleWalletRequested`

Fires when the viewer presses Add to Apple Wallet. The host owns the resulting Apple Wallet flow.

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

**Signature:** `(options: Partial<WhopCardElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class               | Targets                                  |
| ------------------- | ---------------------------------------- |
| `.whop-CardSurface` | The card, controls, and revealed details |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-CardSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: { classes: { 'whop-CardSurface': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.
