> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CardTransactionsElement

> Every card transaction on an account, as the sortable table the dashboard shows: date, merchant, amount, cashback, status, card and cardholder, with filters for status, card and cardholder. A host can seed any of those three so the table opens on a narrowed view the reader can still widen. Card and cardholder narrow the read itself; search and status narrow the rows already loaded, so the count and total describe the current view rather than the account's lifetime. Selecting a row reports it — the element never opens a drawer of its own.

Mounts inside [`Cards`](/elements/upcoming/wallet/cards), in [`Wallet`](/elements/upcoming/wallet/overview). `accountId` and `accessToken` come from `Wallet`. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/cards-cardTransactions">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, Cards, CardTransactionsElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <Cards>
                  <CardTransactionsElement onTransactionSelected={(e) => console.log(e)} />
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
          cards.create('cardTransactions', { onTransactionSelected: (e) => console.log(e) }).mount('#wallet-cards-cardTransactions');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:cards/cardTransactions" data-whop-elements-version="" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="accessToken" type="string">
  A scoped token for the reads. Rows and card labels need `payout:account:read`; cardholder names also need `company:authorized_user:read`. Omitted, calls carry the viewer's own session, which only answers same-origin.
</ResponseField>

<ResponseField name="enabled" type="boolean">
  Off, the table performs no reads and renders empty. Defaults to `true`.
</ResponseField>

<ResponseField name="cardIds" type="string[]">
  Restrict the table to these cards. Empty shows every card on the account. Defaults to `[]`.
</ResponseField>

<ResponseField name="cardholderIds" type="string[]">
  Restrict the table to these cardholders. Empty shows every cardholder on the account. Defaults to `[]`.
</ResponseField>

<ResponseField name="statuses" type="string[]">
  Restrict the table to these statuses — `completed`, `pending`, `declined` or `reversed`. Anything else is ignored. Empty shows every status. Defaults to `[]`.
</ResponseField>

<ResponseField name="showCardholders" type="boolean">
  Show the cardholder column and its filter. Off for an account whose cards all belong to one person. Defaults to `true`.
</ResponseField>

<ResponseField name="pageSize" type="number">
  How many transactions to read per page. Defaults to `50`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onTransactionSelected`

A row was pressed. Open your own detail surface in answer to this — the element never navigates.

**Signature:** `((payload: { transactionId: string; }) => void)`

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

Re-read the transactions, for a host that just changed something on the account.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<CardTransactionsElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                           | Targets                     |
| ------------------------------- | --------------------------- |
| `.whop-CardTransactionsSurface` | The card transactions table |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-CardTransactionsSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-CardTransactionsSurface': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.
