> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CardsChartElement

> A business account's card spend over time, using the same interactive bar chart as Whop's cards dashboard. The period picker changes the mounted chart and emits an event so the host can persist the selection.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Cards`](/elements/beta/wallet/cards), in [`Wallet`](/elements/beta/wallet/overview). `accountId` and `accessToken` come from `Wallet`. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/cards-cardsChart">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, Cards, CardsChartElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <Cards>
                  <CardsChartElement onPeriodChanged={(e) => console.log(e)} onDateSelected={(e) => console.log(e)} />
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
          cards.create('cardsChart', {
            onPeriodChanged: (e) => console.log(e),
            onDateSelected: (e) => console.log(e)
          }).mount('#wallet-cards-cardsChart');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:cards/cardsChart" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="accessToken" type="string">
  A scoped token for card transaction reads. Needs `payout:account:read`. Omitted, calls carry the viewer's own session, which only answers same-origin.
</ResponseField>

<ResponseField name="enabled" type="boolean">
  Off, the chart performs no reads and renders an empty plot. Defaults to `true`.
</ResponseField>

<ResponseField name="period" type="&#x22;7d&#x22; | &#x22;1m&#x22; | &#x22;3m&#x22; | &#x22;ytd&#x22; | &#x22;1y&#x22;">
  The initial reporting window: `7d`, `1m`, `3m`, `ytd`, or `1y`. Defaults to `"1m"`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onPeriodChanged`

The viewer selected a new reporting window. The chart has already updated itself.

**Signature:** `((payload: { period: "7d" | "1m" | "3m" | "ytd" | "1y"; }) => void)`

### `onDateSelected`

The viewer selected or cleared a daily bar. Dates use `YYYY-MM-DD` in UTC.

**Signature:** `((payload: { date: string | null; }) => void)`

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

Re-fetch the selected spend window.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<CardsChartElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                     | Targets                                                      |
| ------------------------- | ------------------------------------------------------------ |
| `.whop-CardsChartSurface` | The total card spend headline, period picker, and daily bars |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-CardsChartSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-CardsChartSurface': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.
