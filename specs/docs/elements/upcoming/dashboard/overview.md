> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Dashboard

> An account's own dashboard surfaces, embedded on your site. Scope it to an account with `accountId`, then mount `paymentsTable` for the account's payments, `paymentDetail` for one payment, `required-actions` for the outstanding-action banners Whop's own dashboard shows above the balance — identity verification, deposits, tax, and the rest — and `verification` for the identity-only nudge. Every surface reads with the same credential, so two side by side always show the same account. The banners render nothing once the account has nothing outstanding, so they can sit permanently in a layout, and they report the presses they cannot answer themselves — Add money and Verify — so the host mounts its own deposit or verification flow, such as the `wallet` controller's `deposit` element or the `verifications` controller's `kyc` element.

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

<div data-whop-demo-shell style={{ position: "relative", minHeight: "480px", transition: "min-height 200ms ease" }}>
  <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

  <div data-whop-demo-native="playground:dashboard" data-whop-elements-version="" style={{ position: "relative" }} />
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
    <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
    <script type="module">
      const dashboard = window.WhopElements().dashboard.create({ /* options */ });
    </script>
    ```
  </CodeGroup>
</div>

## Options

Pass these to `whop.dashboard.create({ … })`, or as props on `<Dashboard>` in React.

<ResponseField name="accessToken" type="string">
  A scoped token every surface under this handle reads with. Mint one token for the whole handle on your server with `POST /api/v1/access_tokens`, and set a fresh one with `update({ accessToken })` before it expires. Reading payments needs `payment:basic:read`; buyer emails additionally need `member:email:read`, the customer journey `member:basic:read`, the tracking column `shipment:basic:read`, and the product and plan names on a payment `access_pass:basic:read` and `plan:basic:read`. The banners need `payout:account:read`, and starting verification from the action bar `identity:write`. Omitted, the reads carry the viewer's own session, which only answers same-origin.
</ResponseField>

<ResponseField name="accountId" type="string" required>
  Account or user ID whose records these surfaces read. Account IDs are prefixed `biz_`; user IDs are prefixed `user_`, can read only the viewer's own outstanding actions, and have no payments.
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
  <Card title="RequiredActionsElement" href="/elements/upcoming/dashboard/required-actions">
    The outstanding-action banners from Whop's balance dashboard — identity verification, deposits, tax, and the rest — in the same order the API returns them. An account with nothing outstanding renders nothing at all, so the element can sit permanently in a layout. Copy comes from the API. Pressing Verify starts a hosted identity session and leaves for it; Add money reports `depositRequested` and stays put, so the host mounts its own deposit flow — the `wallet` controller's `deposit` element, say; every other button follows the action's own link. Reads with the Dashboard handle's `accessToken`, which needs `payout:account:read`, plus `identity:write` to start verification. A failed read renders nothing rather than an error — a banner should never become the loudest thing on someone else's page.
  </Card>

  <Card title="VerificationElement" href="/elements/upcoming/dashboard/verification">
    A banner asking the account holder to verify their identity, shown only while verification is outstanding — an account that has already verified renders nothing at all, so the element can sit permanently in a layout. The headline and status messages come from the API, with a shorter description when inviting the account holder to start verification, so they track the account's actual state: an unstarted account is invited to unlock cards and payouts, one under review reads as pending, and a failed or flagged one says so. Pressing the button reports `verificationRequested` and stays put, so the host mounts its own verification — the `verifications` controller's `kyc` element, say. Reads with the Dashboard handle's `accessToken`, which needs `payout:account:read`. A failed read renders nothing rather than an error — a nudge should never become the loudest thing on the page.
  </Card>

  <Card title="PaymentsTableElement" href="/elements/upcoming/dashboard/paymentsTable">
    The dashboard payments table with status cards, search, filters, sorting, row selection, CSV export, column settings, and pagination. Reads all payment pages to compute complete counts and filter locally; intended for accounts with modest payment histories. Customer details and refunds are handed to your application through events.
  </Card>

  <Card title="PaymentDetailElement" href="/elements/upcoming/dashboard/paymentDetail">
    A payment detail page with the dashboard breakdown, activity, customer, details, and customer journey. Each section can be hidden. Reads payment:basic:read; customer email needs member:email:read and journey needs member:basic:read. Action events let your application confirm and authorize changes; this element never refunds, retries, or voids a payment itself.
  </Card>
</CardGroup>
