> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Reports

> Balance history and the financial activity behind it, including CSV exports. Account statements are available in the Whop dashboard.

Mounts inside [`Wallet`](/elements/upcoming/wallet/overview). Create it to get a handle, then mount its elements on that handle. Call `destroy()` to remove the sub-controller. Create it again to get a fresh handle.

## Preview

A live, interactive demo of this sub-controller's default arrangement with example data:

<div data-whop-demo-shell style={{ position: "relative", minHeight: "480px", transition: "min-height 200ms ease" }}>
  <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

  <div data-whop-demo-native="unit:reports" data-whop-elements-version="" style={{ position: "relative" }} />
</div>

<div data-whop-usage="wallet/reports">
  <CodeGroup>
    ```tsx React theme={null}
    import { WhopElements, Wallet, Reports, BalanceReportElement, ReportActivityElement } from "@whop/elements-react";
    import { loadWhop } from "@whop/elements";

    function Example() {
      return (
        <WhopElements elements={loadWhop()}>
          <Wallet /* options */>
            <Reports>
              <BalanceReportElement />
              <ReportActivityElement />
            </Reports>
          </Wallet>
        </WhopElements>
      );
    }
    ```

    ```html JavaScript theme={null}
    <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
    <script type="module">
      const wallet = window.WhopElements().wallet.create({ /* options */ });
      const reports = wallet.create('reports', { /* options */ });
      reports.create('balanceReport').mount('#wallet-reports-balanceReport');
      reports.create('reportActivity').mount('#wallet-reports-reportActivity');
    </script>
    ```
  </CodeGroup>
</div>

## Options

Pass these to `wallet.create('reports', { … })`, or as props on `<Reports>` in React. Parent-injected props never appear here.

*Reports takes no options.*

## Methods

Call these on the sub handle from `wallet.create('reports', { … })`.

### `update`

Merges new props and callbacks into the sub-controller.

**Signature:** `(options: Partial<ReportsSubOptions>) => void`

### `destroy`

Destroys the sub-controller and its elements, then frees its exclusive slot. A later `create("reports")` starts fresh.

**Signature:** `() => void`

## Elements

The elements this sub-controller mounts. Each has its own page:

<CardGroup cols={2}>
  <Card title="BalanceReportElement" href="/elements/upcoming/wallet/reports-balanceReport">
    Balance history with date, timezone, and currency controls, starting and ending balances, and money-in and money-out breakdowns. Drill into activity and export it without leaving the element.
  </Card>

  <Card title="ReportActivityElement" href="/elements/upcoming/wallet/reports-reportActivity">
    Financial activity with date, currency, direction, and movement filters, pagination, and CSV export. Mount independently or open it from the balance report.
  </Card>
</CardGroup>
