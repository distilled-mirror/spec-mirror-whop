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

# RfiElement

> Everything the account still owes compliance, and the forms to answer it. Each row is one group of requirements — grouped by the system that asked, because each relays to its provider once its own items are answered — and pressing it opens that group’s form in place. Rows already with a reviewer are shown but not answerable. Renders an all-clear once nothing is outstanding, so it can sit permanently in a settings page.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Verifications`](https://docs.whop.com/elements/latest/verifications/overview). Pass props and callbacks through the create options or React props.

```tsx React theme={null}
import { WhopElements, Verifications, RfiElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Verifications /* options */>
        <RfiElement onSubmitted={(e) => console.log(e)} onCompleted={(e) => console.log(e)} onLoadFailed={(e) => console.log(e)} />
      </Verifications>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const verifications = window.WhopElements().verifications.create({ /* options */ });
  verifications.create('rfi', {
    onSubmitted: (e) => console.log(e),
    onCompleted: (e) => console.log(e),
    onLoadFailed: (e) => console.log(e)
  }).mount('#verifications-rfi');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/verifications/overview#playground).

## Props

**hideUnderReview** `boolean`

Hide the rows that are already with a reviewer, leaving only what the viewer can act on. Defaults to `false`.

## Events

Pass callbacks in the create options or React props.

### `onSubmitted`

One group of requirements was answered. Fires per group, not once per field.

**payload** `object`

**properties**

**source** `string` *required*

One of `"identity"`, `"payout"`, `"audit"`, `"card_issuing"`, `"bank"`, `"ads"`, `"application"`, or `"airwallex"`.

**itemIds** `string[]` *required*

### `onCompleted`

Nothing is outstanding any more. Fires when the element first observes an empty list, including on mount.

**payload** `object`

**properties**

**accountId** `string` *required*

### `onLoadFailed`

The outstanding requests could not be read, or answering one failed.

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

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<RfiElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-RfiRow` | One request row: what is being asked, what it affects, and its action |
| `.whop-RfiSurface` | The outstanding-requests surface |

```ts theme={null}
const verifications = whop.verifications.create({
  appearance: {
    classes: {
      'whop-RfiRow': { borderRadius: '8px', fontWeight: '600' },
      'whop-RfiSurface': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

verifications.update({
  appearance: { classes: { 'whop-RfiRow': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Verifications>`. Set it globally with `WhopElements({ appearance })`.


## Related topics

- [Verifications](https://docs.whop.com/elements/latest/verifications/overview.md)
- [Chat Element](https://docs.whop.com/developer/guides/chat/chat-element.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
