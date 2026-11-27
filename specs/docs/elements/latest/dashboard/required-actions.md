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

# RequiredActionsElement

> The outstanding-action banners from Whop's balance dashboard — identity verification, deposits, tax, and the rest — in the same order the API returns them. An account with nothing outstanding renders nothing at all, so the element can sit permanently in a layout. Copy comes from the API. Pressing Verify starts a hosted identity session and leaves for it; Add money reports `depositRequested` and stays put, so the host mounts its own deposit flow — the `wallet` controller's `deposit` element, say; every other button follows the action's own link. Reads with the Dashboard handle's `accessToken`, which needs `payout:account:read`, plus `identity:write` to start verification. A failed read renders nothing rather than an error — a banner should never become the loudest thing on someone else's page.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Dashboard`](https://docs.whop.com/elements/latest/dashboard/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

```tsx React theme={null}
import { WhopElements, Dashboard, RequiredActionsElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Dashboard /* options */>
        <RequiredActionsElement onActionRequested={(e) => console.log(e)} onIdentityVerificationRequested={(e) => console.log(e)} onDepositRequested={(e) => console.log(e)} />
      </Dashboard>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const dashboard = window.WhopElements().dashboard.create({ /* options */ });
  dashboard.create('required-actions', {
    onActionRequested: (e) => console.log(e),
    onIdentityVerificationRequested: (e) => console.log(e),
    onDepositRequested: (e) => console.log(e)
  }).mount('#dashboard-required-actions');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/dashboard/overview#playground).

## Props

**kind** `"individual" | "business"`

Which verification the Verify button starts. `individual` (KYC) is what unlocks payouts and a Whop card. `business` (KYB) covers that and additionally unlocks financing and business cards — use it for a company that will need those. Defaults to `"individual"`.

## Events

Pass callbacks in the create options or React props.

### `onActionRequested`

The viewer pressed a banner button. Fires for every action, including ones the element also handles itself.

**payload** `object`

**properties**

**action** `string` *required*

**values**

One of `"deposit_funds"`, `"review_held_payments"`, `"submit_information_request"`, `"update_automatic_withdrawal_method"`, `"reauthorize_payout_methods"`, `"update_payout_method"`, `"update_payout_profile"`, `"card_usage_review"`, `"verify_identity"`, `"scale_account_setup"`, `"sign_formation_documents"`, `"configure_tax_remitter"`, `"add_vat_registration"`, `"confirm_tax_details"`, `"accept_payout_terms"`, `"enable_two_factor_authentication"`, or `"respond_to_abuse_report"`.

**accountId** `string` *required*

### `onIdentityVerificationRequested`

The viewer pressed Verify on a `verify_identity` action the element can start. The element opens identity verification itself — this fires alongside, so a host can record the trip.

**payload** `object`

**properties**

**accountId** `string` *required*

### `onDepositRequested`

The viewer pressed Add money. The element never opens a deposit flow itself — mount your own, such as the `wallet` controller’s `deposit` element, in answer to this.

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

Re-read the account. Call it when the viewer returns from verifying or depositing so the banners reflect the new state without waiting for the cache to expire.

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

**options** `Partial\<RequiredActionsElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-RequiredActions` | The outstanding-action banners |

```ts theme={null}
const dashboard = whop.dashboard.create({
  appearance: {
    classes: {
      'whop-RequiredActions': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

dashboard.update({
  appearance: {
    classes: { 'whop-RequiredActions': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Dashboard>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Dashboard](https://docs.whop.com/elements/latest/dashboard/overview.md)
- [KycElement](https://docs.whop.com/elements/latest/verifications/kyc.md)
- [PaymentElement](https://docs.whop.com/elements/latest/payments/payment.md)
