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

# ConvertElement

> Converts an account's USD balance to Gold, Coinbase Wrapped Bitcoin, or a stock token, and back, through Whop's public swaps API. A cross-origin mount needs an `accessToken` scoped to `company:balance:read` plus `crypto_wallet:swap` or `crypto_wallet:manage`.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Wallet`](https://docs.whop.com/elements/latest/wallet/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

> [!NOTE]
>
> You can mount this element **inline** (`create`) or open it as a **modal** overlay (`createOverlay`).

```tsx React theme={null}
import { WhopElements, Wallet, ConvertElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Wallet /* options */>
        <ConvertElement onViewChanged={(e) => console.log(e)} onBalanceChanged={(e) => console.log(e)} onDone={(e) => console.log(e)} />
      </Wallet>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const wallet = window.WhopElements().wallet.create({ /* options */ });
  wallet.create('convert', {
    onViewChanged: (e) => console.log(e),
    onBalanceChanged: (e) => console.log(e),
    onDone: (e) => console.log(e)
  }).mount('#wallet-convert');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/wallet/overview#playground).

## Props

*This element takes no consumer props.*

## Events

Pass callbacks in the create options or React props.

### `onViewChanged`

The visible conversion step changed.

**payload** `object`

**properties**

**view** `"pending" | "input" | "review" | "success"` *required*

### `onBalanceChanged`

A conversion completed or remains pending after the polling window. Refresh account balance and activity.

### `onDone`

The viewer finished or dismissed the conversion flow.

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

Re-fetch the account balance after another surface changes it.

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

**options** `Partial\<ConvertElementProps\>` *required*

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.


## Related topics

- [Wallet](https://docs.whop.com/elements/latest/wallet/overview.md)
- [ActionsElement](https://docs.whop.com/elements/latest/wallet/actions.md)
- [Chat Element](https://docs.whop.com/developer/guides/chat/chat-element.md)
