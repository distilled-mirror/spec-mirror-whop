> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# ActivityDetailElement

> A standalone detail drawer for one movement. Point it at a prefetched financial-activity row, a ledger activity ID, or a card transaction ID — a prefetched row takes precedence, otherwise the element retrieves the record itself. A card transaction opens the card receipt: merchant, amount, status, the card it was charged to, its cardholder on a company account, the settlement date, category, currency conversion and cashback.

Opens as a modal from [`Wallet`](/elements/upcoming/wallet/overview): `wallet.createOverlay('activityDetail')`. Pass props and callbacks in the create options.

<Note>This element is **modal-only**. Open it with `createOverlay`; it has no inline mount.</Note>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/activityDetail">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, ActivityDetailElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <ActivityDetailElement onCardSelected={(e) => console.log(e)} onCardholderSelected={(e) => console.log(e)} onCardTransactionIssueRequested={(e) => console.log(e)} />
              </Wallet>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const wallet = window.WhopElements().wallet.create({ /* options */ });
          wallet.createOverlay('activityDetail', {
            onCardSelected: (e) => console.log(e),
            onCardholderSelected: (e) => console.log(e),
            onCardTransactionIssueRequested: (e) => console.log(e)
          }).open();
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:wallet/activityDetail" data-whop-elements-version="" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="activity" type="LedgerActivity | null">
  Optional prefetched ledger activity row. When provided, the element renders it without another request. Defaults to `null`.
</ResponseField>

<ResponseField name="activityId" type="string | null">
  Optional ledger activity ID to retrieve when no prefetched activity row is provided. Defaults to `null`.
</ResponseField>

<ResponseField name="cardTransactionId" type="string | null">
  Optional card transaction ID (`citx_`) to retrieve and show as a card receipt — what a card table hands back when a row is pressed. Takes precedence over `activityId`, and is ignored when a prefetched row is passed. Defaults to `null`.
</ResponseField>

<ResponseField name="showCardholder" type="boolean">
  Show the cardholder row on a card receipt. Company accounts only — a personal wallet has one cardholder, so the row never appears there. Reading the name needs `company:authorized_user:read`; without it the row reads Unavailable. Defaults to `true`.
</ResponseField>

<ResponseField name="showCardTransactionReportIssueAction" type="boolean">
  Show the report-an-issue action for eligible card transactions. The action emits an event for the host. Defaults to `false`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onCardSelected`

The card row was clicked.

**Signature:** `((payload: { cardId: string; }) => void)`

### `onCardholderSelected`

The cardholder row was clicked. Open your own profile surface for that user.

**Signature:** `((payload: { userId: string; }) => void)`

### `onCardTransactionIssueRequested`

Report an issue was clicked. Open the host-owned card dispute flow.

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

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<ActivityDetailElementProps>) => void`

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.
