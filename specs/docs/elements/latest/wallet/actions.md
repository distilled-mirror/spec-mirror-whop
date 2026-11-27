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

# ActionsElement

> The account action row from Whop's balance dashboard. Deposit, Send, Withdraw, and Convert open the Wallet controller's built-in overlays. Accept opens Whop's checkout-link creator for a business account or company creation for a personal account. Each button also emits its requested event so the embedding page can observe the action.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). Pass props and callbacks through the create options or React props.

```tsx React theme={null}
import { WhopElements, Wallet, ActionsElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <ActionsElement onDepositRequested={(e) => console.log(e)} onAcceptRequested={(e) => console.log(e)} onSendRequested={(e) => console.log(e)} onWithdrawRequested={(e) => console.log(e)} onConvertRequested={(e) => console.log(e)} />
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  wallet.create('actions', {
    onDepositRequested: (e) => console.log(e),
    onAcceptRequested: (e) => console.log(e),
    onSendRequested: (e) => console.log(e),
    onWithdrawRequested: (e) => console.log(e),
    onConvertRequested: (e) => console.log(e)
  }).mount('#wallet-actions');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

## Props

**showDeposit** `boolean`

Show the Deposit button. Defaults to `true`.

**showAccept** `boolean`

Show the Accept button. Defaults to `true`.

**showSend** `boolean`

Show the Send button. Defaults to `true`.

**showWithdraw** `boolean`

Show the Withdraw button. Off by default so an existing row keeps the buttons it already had. Defaults to `false`.

**showConvert** `boolean`

Show the Convert button. Defaults to `true`.

**depositDisabled** `boolean`

Disable the Deposit button while keeping it visible. Defaults to `false`.

**acceptDisabled** `boolean`

Disable the Accept button while keeping it visible. Defaults to `false`.

**sendDisabled** `boolean`

Disable the Send button while keeping it visible. Defaults to `false`.

**withdrawDisabled** `boolean`

Disable the Withdraw button while keeping it visible. Defaults to `false`.

**convertDisabled** `boolean`

Disable the Convert button while keeping it visible. Defaults to `false`.

## Events

Pass callbacks in the create options or React props.

### `onDepositRequested`

The Deposit button was pressed and the Wallet controller opened its deposit overlay.

### `onAcceptRequested`

The Accept button was pressed and the Wallet controller opened the corresponding Whop setup page.

### `onSendRequested`

The Send button was pressed and the Wallet controller opened its send overlay.

### `onWithdrawRequested`

The Withdraw button was pressed and the Wallet controller opened its withdraw overlay.

### `onConvertRequested`

The Convert button was pressed and the Wallet controller opened its convert overlay.

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

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<ActionsElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-Actions` | The row containing the Deposit, Accept, Send, Convert, and Withdraw buttons. |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-Actions': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: { classes: { 'whop-Actions': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Wallet](https://docs.whop.com/elements/latest/wallet/overview.md)
- [RequiredActionsElement](https://docs.whop.com/elements/latest/dashboard/required-actions.md)
- [KycElement](https://docs.whop.com/elements/latest/verifications/kyc.md)
