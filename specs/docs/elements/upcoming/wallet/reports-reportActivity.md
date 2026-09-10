> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# ReportActivityElement

> Financial activity with date, currency, direction, and movement filters, pagination, and CSV export. Mount independently or open it from the balance report.

Mounts inside [`Reports`](/elements/upcoming/wallet/reports), in [`Wallet`](/elements/upcoming/wallet/overview). `accountId` and `accessToken` come from `Wallet`. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/reports-reportActivity">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, Reports, ReportActivityElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <Reports>
                  <ReportActivityElement onExportReady={(e) => console.log(e)} />
                </Reports>
              </Wallet>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const wallet = window.WhopElements().wallet.create({ /* options */ });
          const reports = wallet.create('reports', { /* options */ });
          reports.create('reportActivity', { onExportReady: (e) => console.log(e) }).mount('#wallet-reports-reportActivity');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:reports/reportActivity" data-whop-elements-version="" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="filters" type="ReportActivityFilters">
  Initial filters. Changing these resets the table to the requested activity. Defaults to `{}`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onExportReady`

The CSV is ready; downloadUrl also appears as a clickable link.

**Signature:** `((payload: ReportExportReady) => void)`

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

Refresh the report data after activity changes.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<ReportActivityElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                  | Targets                                |
| ---------------------- | -------------------------------------- |
| `.whop-ReportActivity` | The report activity table and filters. |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-ReportActivity': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-ReportActivity': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.
