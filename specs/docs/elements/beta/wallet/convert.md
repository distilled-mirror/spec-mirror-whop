> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# ConvertElement

> Converts an account's USD balance to Gold or Coinbase Wrapped Bitcoin, and back, through Whop's public swaps API. A cross-origin mount needs an `accessToken` scoped to `company:balance:read` plus `crypto_wallet:swap` or `crypto_wallet:manage`.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Wallet`](/elements/beta/wallet/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<Note>You can mount this element **inline** (`create`) or open it as a **modal** overlay (`createOverlay`).</Note>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/convert">
      <CodeGroup>
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
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const wallet = window.WhopElements().wallet.create({ /* options */ });
          wallet.create('convert', {
            onViewChanged: (e) => console.log(e),
            onBalanceChanged: (e) => console.log(e),
            onDone: (e) => console.log(e)
          }).mount('#wallet-convert');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:wallet/convert" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/wallet/overview#playground).</p>
  </div>
</div>

## Props

*This element takes no consumer props.*

## Events

Pass callbacks in the create options or React props.

### `onViewChanged`

The visible conversion step changed.

**Signature:** `((payload: { view: "pending" | "input" | "review" | "success"; }) => void)`

### `onBalanceChanged`

A conversion completed or remains pending after the polling window. Refresh account balance and activity.

**Signature:** `((payload: Record<string, never>) => void)`

### `onDone`

The viewer finished or dismissed the conversion flow.

**Signature:** `((payload: Record<string, never>) => void)`

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

Re-fetch the account balance after another surface changes it.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<ConvertElementProps>) => void`

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.
