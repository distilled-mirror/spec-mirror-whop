> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## Agent Instructions
> Treat these docs and the Whop MCP server as the source of truth for Whop API behavior. Don't infer endpoint behavior from marketing pages.
> MCP server: connect https://mcp.whop.com/mcp (streamable HTTP, browser sign-in, no API key in the client config) to search these docs and read or change live Whop data. Setup: https://docs.whop.com/developer/guides/ai_and_mcp.md
> Whop CLI: install with `curl -fsSL https://whop.com/install.sh | sh` to scaffold, run, and deploy Whop apps and websites. It acts on production, not the sandbox. Overview: https://docs.whop.com/cli/overview.md Agent mode: https://docs.whop.com/cli/agent-mode.md
> Hosting: Whop serves a site or app from the build `whop apps deploy` uploads, with rollbacks, secrets, and server logs. Guide: https://docs.whop.com/developer/websites/hosting.md
> Plugins: in Claude Code, run `/plugin marketplace add whopio/plugins`, then `/plugin install whop@whop`. Guide: https://docs.whop.com/developer/guides/plugins/claude-code.md Every agent tool: https://docs.whop.com/developer/ai/overview.md
> Environments: build and test in the sandbox (API `https://sandbox-api.whop.com/api/v1`, keys at https://sandbox.whop.com/dashboard/developer, SDK environment `Sandbox`; no real money). Production (API `https://api.whop.com/api/v1`, keys at https://whop.com/dashboard/developer, SDK environment `Production`, the SDK default) moves real money, so use it only at launch. The two share no accounts, keys, products, or webhooks, and their keys look identical: never mix them. Sandbox guide: https://docs.whop.com/developer/guides/sandbox.md Comparison: https://docs.whop.com/developer/quickstart.md
> API: send `Authorization: Bearer <API_KEY>` from your server only, and pin a dated version with the `Api-Version-Date` header. OpenAPI spec: https://api.whop.com/api/v1/openapi.json
> Which API: build on the Current API, whose pages live under https://docs.whop.com/api-reference/ (start at https://docs.whop.com/api-reference/overview.md). Pages under /api-reference/legacy/ are the Legacy API. Use one only when https://docs.whop.com/api-reference/stability.md lists the resource as Legacy-only.

# PaymentsTableElement

> The dashboard payments table with status cards, search, filters, sorting, row selection, CSV export, column settings, and pagination. Reads all payment pages to compute complete counts and filter locally; intended for accounts with modest payment histories. Customer details and refunds are handed to your application through events.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Dashboard`](https://docs.whop.com/elements/latest/dashboard/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

```tsx React theme={null}
import { WhopElements, Dashboard, PaymentsTableElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Dashboard /* options */>
        <PaymentsTableElement onPaymentSelected={(e) => console.log(e)} onInvoiceRequested={(e) => console.log(e)} onUserSelected={(e) => console.log(e)} onRefundRequested={(e) => console.log(e)} onSettingsRequested={(e) => console.log(e)} />
      </Dashboard>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const dashboard = window.WhopElements().dashboard.create({ /* options */ });
  dashboard.create('paymentsTable', {
    onPaymentSelected: (e) => console.log(e),
    onInvoiceRequested: (e) => console.log(e),
    onUserSelected: (e) => console.log(e),
    onRefundRequested: (e) => console.log(e),
    onSettingsRequested: (e) => console.log(e)
  }).mount('#dashboard-paymentsTable');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/dashboard/overview#playground).

## Props

**showActions** `boolean`

Show the header action menu, Export button, and table settings button. Defaults to `true`.

**showRowActions** `boolean`

Show the three-dot action menu on each payment row. Defaults to `true`.

**showStatusTabs** `boolean`

Show the six payment status boxes above the filters. Defaults to `true`.

**showTracking** `boolean`

Show the tracking column and filter for physical-product accounts. Tracking data requires `shipment:basic:read`. Defaults to `true`.

## Events

Pass callbacks in the create options or React props.

### `onPaymentSelected`

Open payment details in your application.

**payload** `object`

**properties**

**paymentId** `string` *required*

### `onInvoiceRequested`

Open the invoice in your application.

**payload** `object`

**properties**

**paymentId** `string` *required*

### `onUserSelected`

Open the buyer in your application.

**payload** `object`

**properties**

**userId** `string` *required*

### `onRefundRequested`

The viewer requested a refund. Your application must confirm and authorize it server-side. Refund controls are enabled only when this callback is provided.

**payload** `object`

**properties**

**paymentIds** `string[]` *required*

### `onSettingsRequested`

Open the account’s payment settings in your application.

### `onLoaderStart`

Runs after the loading skeleton first paints and before `onReady`.

### `onReady`

Runs after the element's first complete paint.

### `onError`

Runs when the element fails to load or crashes. The fallback remains visible. Use `code` for programmatic handling. `sourceKey` identifies a failed host-state source.

**e** `object`

**properties**

**message** `string` *required*

**code** `string`

**sourceKey** `string`

## Methods

Call these on the handle returned by `create`, or through a React `ref`.

### `refresh`

Reload payments after a refund or another change.

**Returns** `Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<PaymentsTableElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-PaymentsTable` | The complete payments table |

```ts theme={null}
const dashboard = whop.dashboard.create({
  appearance: {
    classes: {
      'whop-PaymentsTable': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

dashboard.update({
  appearance: { classes: { 'whop-PaymentsTable': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Dashboard>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Dashboard](https://docs.whop.com/elements/latest/dashboard/overview.md)
- [Payments](https://docs.whop.com/elements/latest/payments/overview.md)
- [PaymentElement](https://docs.whop.com/elements/latest/payments/payment.md)
