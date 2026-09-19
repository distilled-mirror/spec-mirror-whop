> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# PaymentDetailElement

> A payment detail page with the dashboard breakdown, activity, customer, details, and customer journey. Each section can be hidden. Reads payment:basic:read; customer email needs member:email:read and journey needs member:basic:read. Action events let your application confirm and authorize changes; this element never refunds, retries, or voids a payment itself.

<Info>This page documents `@whop/elements@1.0.0-beta.6` and `@whop/elements-react@1.0.0-beta.6`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Dashboard`](/elements/beta/dashboard/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="dashboard/paymentDetail">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Dashboard, PaymentDetailElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Dashboard /* options */>
                <PaymentDetailElement onRefundRequested={(e) => console.log(e)} onRetryRequested={(e) => console.log(e)} onVoidRequested={(e) => console.log(e)} onInvoiceRequested={(e) => console.log(e)} onCustomerSelected={(e) => console.log(e)} onMessageRequested={(e) => console.log(e)} />
              </Dashboard>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const dashboard = window.WhopElements().dashboard.create({ /* options */ });
          dashboard.create('paymentDetail', {
            onRefundRequested: (e) => console.log(e),
            onRetryRequested: (e) => console.log(e),
            onVoidRequested: (e) => console.log(e),
            onInvoiceRequested: (e) => console.log(e),
            onCustomerSelected: (e) => console.log(e),
            onMessageRequested: (e) => console.log(e)
          }).mount('#dashboard-paymentDetail');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:dashboard/paymentDetail" data-whop-elements-version="1.0.0-beta.6" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/dashboard/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="showActions" type="boolean">
  Show the top-right Refund button and action menu. Mutating actions require event handlers. Defaults to `true`.
</ResponseField>

<ResponseField name="paymentId" type="string">
  The payment to display, prefixed pay\_. Required to load a payment. Defaults to `""`.
</ResponseField>

<ResponseField name="showBreakdown" type="boolean">
  Show the payment amount, fees, and net amount. Defaults to `true`.
</ResponseField>

<ResponseField name="showActivity" type="boolean">
  Show the payment activity timeline. Defaults to `true`.
</ResponseField>

<ResponseField name="showCustomer" type="boolean">
  Show customer name, username, email, phone, and billing address when permitted. Defaults to `true`.
</ResponseField>

<ResponseField name="showDetails" type="boolean">
  Show product, plan, payment method, and payment identifiers. Defaults to `true`.
</ResponseField>

<ResponseField name="showJourney" type="boolean">
  Show the customer journey. Reads the account-scoped Events API only while visible. Defaults to `true`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onRefundRequested`

Confirm and authorize a refund in your application. Enabled only when refundable and a handler is provided.

**Signature:** `((payload: { paymentId: string; }) => void)`

### `onRetryRequested`

Confirm and authorize another charge attempt in your application.

**Signature:** `((payload: { paymentId: string; }) => void)`

### `onVoidRequested`

Confirm and authorize voiding this payment in your application.

**Signature:** `((payload: { paymentId: string; }) => void)`

### `onInvoiceRequested`

View or download the invoice in your application.

**Signature:** `((payload: { paymentId: string; }) => void)`

### `onCustomerSelected`

Open the customer in your application.

**Signature:** `((payload: { userId: string; memberId: string | null; }) => void)`

### `onMessageRequested`

Open your customer messaging UI.

**Signature:** `((payload: { userId: string; }) => void)`

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

Reload the payment and its fees after a host action completes.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<PaymentDetailElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                 | Targets                 |
| --------------------- | ----------------------- |
| `.whop-PaymentDetail` | The payment detail page |

```ts theme={null}
const dashboard = whop.dashboard.create({
  appearance: {
    classes: {
      'whop-PaymentDetail': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

dashboard.update({
  appearance: { classes: { 'whop-PaymentDetail': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Dashboard>`. Set it globally with `WhopElements({ appearance })`.
