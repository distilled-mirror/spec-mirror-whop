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

# CapabilitiesElement

> The account's verification standing: whether individual and business verification are done, and which capabilities that unlocks. Read-only — pressing a verify button reports `verificationRequested` and stays put, so the host mounts its own flow, such as this namespace's `kyc` element. Both halves can be hidden, so a host that only wants the capability list, or only the two verification rows, can drop the other.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Verifications`](https://docs.whop.com/elements/latest/verifications/overview). Pass props and callbacks through the create options or React props.

```tsx React theme={null}
import { WhopElements, Verifications, CapabilitiesElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Verifications /* options */>
        <CapabilitiesElement onVerificationRequested={(e) => console.log(e)} />
      </Verifications>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const verifications = window.WhopElements().verifications.create({ /* options */ });
  verifications.create('capabilities', { onVerificationRequested: (e) => console.log(e) }).mount('#verifications-capabilities');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/verifications/overview#playground).

## Props

**hideStatus** `boolean`

Hide the individual and business verification rows, leaving only the capabilities. Defaults to `false`.

**hideCapabilities** `boolean`

Hide the capabilities list, leaving only the two verification rows. Defaults to `false`.

**hidePayouts** `boolean`

Hide the Payouts capability row. Defaults to `false`. Since `v1.9.0`.

**hideBankDeposits** `boolean`

Hide the Bank deposits capability row. Defaults to `false`. Since `v1.9.0`.

**hideCards** `boolean`

Hide the Whop Card capability row. Defaults to `false`. Since `v1.9.0`.

**hideAds** `boolean`

Hide the Ads capability row. Defaults to `false`. Since `v1.9.0`.

## Events

Pass callbacks in the create options or React props.

### `onVerificationRequested`

The viewer pressed a verify button. The element never navigates — mount your own verification, such as this namespace’s `kyc` element, in answer to this.

**payload** `object`

**properties**

**accountId** `string` *required*

**kind** `"individual" | "business"` *required*

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

**options** `Partial\<CapabilitiesElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-CapabilityRow` | One capability row: its name, hint and state |
| `.whop-VerificationStatusRow` | One verification row: its name and its action |
| `.whop-VerificationStatusSurface` | The verification status surface |

```ts theme={null}
const verifications = whop.verifications.create({
  appearance: {
    classes: {
      'whop-CapabilityRow': { borderRadius: '8px', fontWeight: '600' },
      'whop-VerificationStatusRow': { borderRadius: '8px', fontWeight: '600' },
      'whop-VerificationStatusSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

verifications.update({
  appearance: { classes: { 'whop-CapabilityRow': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Verifications>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Verifications](https://docs.whop.com/elements/latest/verifications/overview.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
- [PaymentElement](https://docs.whop.com/elements/latest/payments/payment.md)
