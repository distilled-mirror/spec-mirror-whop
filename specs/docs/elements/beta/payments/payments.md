> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# PaymentsElement

> The dashboard payments table with status cards, search, filters, sorting, row selection, CSV export, column settings, and pagination. Reads all payment pages to compute complete counts and filter locally; intended for accounts with modest payment histories. Customer details and refunds are handed to your application through events.

<Info>This page documents `@whop/elements@1.0.0-beta.4` and `@whop/elements-react@1.0.0-beta.4`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Payments`](/elements/beta/payments/overview). Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="payments/payments">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Payments, PaymentsElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Payments /* options */>
                <PaymentsElement onPaymentSelected={(e) => console.log(e)} onInvoiceRequested={(e) => console.log(e)} onUserSelected={(e) => console.log(e)} onRefundRequested={(e) => console.log(e)} onSettingsRequested={(e) => console.log(e)} />
              </Payments>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const payments = window.WhopElements().payments.create({ /* options */ });
          payments.create('payments', {
            onPaymentSelected: (e) => console.log(e),
            onInvoiceRequested: (e) => console.log(e),
            onUserSelected: (e) => console.log(e),
            onRefundRequested: (e) => console.log(e),
            onSettingsRequested: (e) => console.log(e)
          }).mount('#payments-payments');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:payments/payments" data-whop-elements-version="1.0.0-beta.4" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/payments/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="showActions" type="boolean">
  Show the header action menu, Export button, and table settings button. Defaults to `true`.
</ResponseField>

<ResponseField name="showRowActions" type="boolean">
  Show the three-dot action menu on each payment row. Defaults to `true`.
</ResponseField>

<ResponseField name="showStatusTabs" type="boolean">
  Show the six payment status boxes above the filters. Defaults to `true`.
</ResponseField>

<ResponseField name="showTracking" type="boolean">
  Show the tracking column and filter for physical-product accounts. Tracking data requires `shipment:basic:read`. Defaults to `true`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onPaymentSelected`

Open payment details in your application.

**Signature:** `((payload: { paymentId: string; }) => void)`

### `onInvoiceRequested`

Open the invoice in your application.

**Signature:** `((payload: { paymentId: string; }) => void)`

### `onUserSelected`

Open the buyer in your application.

**Signature:** `((payload: { userId: string; }) => void)`

### `onRefundRequested`

The viewer requested a refund. Your application must confirm and authorize it server-side. Refund controls are enabled only when this callback is provided.

**Signature:** `((payload: { paymentIds: string[]; }) => void)`

### `onSettingsRequested`

Open the account’s payment settings in your application.

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

Reload payments after a refund or another change.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<PaymentsElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                 | Targets                     |
| --------------------- | --------------------------- |
| `.whop-PaymentsTable` | The complete payments table |

```ts theme={null}
const payments = whop.payments.create({
  appearance: {
    classes: {
      'whop-PaymentsTable': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

payments.update({
  appearance: { classes: { 'whop-PaymentsTable': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.
