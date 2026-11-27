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

# KycElement

> A complete identity-verification flow. It collects individual KYC or business KYB details, starts or resumes the hosted provider session, handles follow-up information and document requests, polls status, and renders the final result without requiring the host to build verification UI.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Verifications`](https://docs.whop.com/elements/latest/verifications/overview). Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()` and `restart()`.

```tsx React theme={null}
import { WhopElements, Verifications, KycElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Verifications /* options */>
        <KycElement onStatusChanged={(e) => console.log(e)} onCompleted={(e) => console.log(e)} onActionRequired={(e) => console.log(e)} onLoadFailed={(e) => console.log(e)} />
      </Verifications>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const verifications = window.WhopElements().verifications.create({ /* options */ });
  verifications.create('kyc', {
    onStatusChanged: (e) => console.log(e),
    onCompleted: (e) => console.log(e),
    onActionRequired: (e) => console.log(e),
    onLoadFailed: (e) => console.log(e)
  }).mount('#verifications-kyc');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/verifications/overview#playground).

## Props

*This element takes no consumer props.*

## Events

Pass callbacks in the create options or React props.

### `onStatusChanged`

The verification entered a new status, including the status found on first mount.

**payload** `object`

**properties**

**verificationId** `string` *required*

**status** `string` *required*

One of `"not_started"`, `"pending"`, `"processing"`, `"manual_review"`, `"approved"`, `"rejected"`, or `"action_required"`.

### `onCompleted`

Verification was approved. Fires once when the element first observes `approved`.

**payload** `object`

**properties**

**verificationId** `string` *required*

### `onActionRequired`

Whop needs follow-up information. The element renders and submits the requested fields itself.

**payload** `object`

**properties**

**verificationId** `string` *required*

**requestedInformation** `ActionRequiredItem[]` *required*

**properties**

**id** `string` *required*

**requirement** `string` *required*

**type** `string` *required*

**label** `string` *required*

**source** `string`

One of `"identity"`, `"payout"`, `"audit"`, `"card_issuing"`, `"bank"`, `"ads"`, `"application"`, or `"airwallex"`.

**optional** `boolean`

**options** `string[]`

**alternative\_document\_label** `string`

**errors** `object[]`

**properties**

**code** `string` *required*

**reason** `string` *required*

### `onLoadFailed`

The initial load, intake submission, follow-up submission, or an explicit restart failed.

**payload** `object`

**properties**

**reason** `"access_denied" | "unavailable"` *required*

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

Fetch the latest verification status immediately. Pending and review states also poll automatically.

**Returns** `Promise<object | null>`

**verificationId** `string` *required*

**status** `string` *required*

One of `"not_started"`, `"pending"`, `"processing"`, `"manual_review"`, `"approved"`, `"rejected"`, or `"action_required"`.

### `restart`

Abandon the current attempt and start a fresh hosted session immediately.

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

**options** `Partial\<KycElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-KycFrame` | The verification panel |
| `.whop-KycSurface` | The complete verification surface — intake, hosted verification handoff, and results |

```ts theme={null}
const verifications = whop.verifications.create({
  appearance: {
    classes: {
      'whop-KycFrame': { borderRadius: '8px', fontWeight: '600' },
      'whop-KycSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

verifications.update({
  appearance: { classes: { 'whop-KycFrame': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Verifications>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Verifications](https://docs.whop.com/elements/latest/verifications/overview.md)
- [CapabilitiesElement](https://docs.whop.com/elements/latest/verifications/capabilities.md)
- [VerificationElement](https://docs.whop.com/elements/latest/dashboard/verification.md)
