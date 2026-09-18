> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Dashboard

> An account's own payment records, embedded on your site. Scope it to an account with `accountId`, then mount `paymentsTable` for the account's payments and `paymentDetail` for one payment. Both read with the same credential, so two surfaces side by side always show the same account.

<Info>This page documents `@whop/elements@1.0.0-beta.5` and `@whop/elements-react@1.0.0-beta.5`.</Info>

*Pre-release, not yet part of a stable release.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

<div data-whop-demo-shell style={{ position: "relative", minHeight: "480px", transition: "min-height 200ms ease" }}>
  <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

  <div data-whop-demo-native="playground:dashboard" data-whop-elements-version="1.0.0-beta.5" style={{ position: "relative" }} />
</div>

<div data-whop-usage="dashboard/playground">
  <CodeGroup>
    ```tsx React theme={null}
    import { WhopElements, Dashboard } from "@whop/elements-react";
    import { loadWhop } from "@whop/elements";

    function Example() {
      return (
        <WhopElements elements={loadWhop()}>
          <Dashboard /* options */>
            {/* mount elements here */}
          </Dashboard>
        </WhopElements>
      );
    }
    ```

    ```html JavaScript theme={null}
    <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
    <script type="module">
      const dashboard = window.WhopElements().dashboard.create({ /* options */ });
    </script>
    ```
  </CodeGroup>
</div>

## Options

Pass these to `whop.dashboard.create({ … })`, or as props on `<Dashboard>` in React.

<ResponseField name="accessToken" type="string">
  A scoped token every surface under this handle reads with. Mint one token for the whole handle on your server with `POST /api/v1/access_tokens`, and set a fresh one with `update({ accessToken })` before it expires. Reading payments needs `payment:basic:read`; buyer emails additionally need `member:email:read`, the customer journey `member:basic:read`, the tracking column `shipment:basic:read`, and the product and plan names on a payment `access_pass:basic:read` and `plan:basic:read`. Omitted, the reads carry the viewer's own session, which only answers same-origin.
</ResponseField>

<ResponseField name="accountId" type="string" required>
  Account ID, prefixed `biz_`, whose payments these surfaces read.
</ResponseField>

<ResponseField name="appearance" type="Appearance">
  Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.
</ResponseField>

<ResponseField name="locale" type="WhopElementsLocale">
  Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**Signature:** `((loading: boolean) => void)`

## Methods

Call these on the Dashboard handle from `whop.dashboard.create({ … })` or `useDashboard()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Signature:** `(options: Partial<DashboardOptions>) => void`

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

**Signature:** `() => void`

## Elements

The elements this group mounts. Each has its own page:

<CardGroup cols={2}>
  <Card title="PaymentsTableElement" href="/elements/beta/dashboard/paymentsTable">
    The dashboard payments table with status cards, search, filters, sorting, row selection, CSV export, column settings, and pagination. Reads all payment pages to compute complete counts and filter locally; intended for accounts with modest payment histories. Customer details and refunds are handed to your application through events.
  </Card>

  <Card title="PaymentDetailElement" href="/elements/beta/dashboard/paymentDetail">
    A payment detail page with the dashboard breakdown, activity, customer, details, and customer journey. Each section can be hidden. Reads payment:basic:read; customer email needs member:email:read and journey needs member:basic:read. Action events let your application confirm and authorize changes; this element never refunds, retries, or voids a payment itself.
  </Card>
</CardGroup>
