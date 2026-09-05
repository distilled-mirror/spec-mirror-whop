> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# RequiredActionsElement

> The outstanding-action banners from Whop's balance dashboard — identity verification, deposits, tax, and the rest — in the same order the API returns them. An account with nothing outstanding renders nothing at all, so the element can sit permanently in a layout. Copy comes from the API. Pressing Verify starts a hosted identity session and leaves for it; Add money asks the Wallet controller to open deposit; every other button follows the action's own link. Needs an `accessToken`. A failed read renders nothing rather than an error — a banner should never become the loudest thing on someone else's page.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Wallet`](/elements/beta/wallet/overview). `accountId` comes from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="wallet/required-actions">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Wallet, RequiredActionsElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Wallet /* options */>
                <RequiredActionsElement onActionRequested={(e) => console.log(e)} onIdentityVerificationRequested={(e) => console.log(e)} onDepositRequested={(e) => console.log(e)} />
              </Wallet>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const wallet = window.WhopElements().wallet.create({ /* options */ });
          wallet.create('required-actions', {
            onActionRequested: (e) => console.log(e),
            onIdentityVerificationRequested: (e) => console.log(e),
            onDepositRequested: (e) => console.log(e)
          }).mount('#wallet-required-actions');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:wallet/required-actions" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/wallet/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="accessToken" type="string">
  A scoped token for the read and for starting verification — needs `payout:account:read` and `identity:write`. Mint it on your server with `POST /api/v1/access_tokens`. Omitted, the calls carry the viewer's own session, which only answers same-origin.
</ResponseField>

<ResponseField name="kind" type="&#x22;individual&#x22; | &#x22;business&#x22;">
  Which verification the Verify button starts. `individual` (KYC) is what unlocks payouts and a Whop card. `business` (KYB) covers that and additionally unlocks financing and business cards — use it for a company that will need those. Defaults to `"individual"`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onActionRequested`

The viewer pressed a banner button. Fires for every action, including ones the element also handles itself.

**Signature:** `((payload: { action: "deposit_funds" | "submit_information_request" | "update_automatic_withdrawal_method" | "reauthorize_payout_methods" | "update_payout_profile" | "card_usage_review" | "verify_identity" | "sign_formation_documents" | "connect_fulfillment_tracker" | "setup_apple_pay_domains" | "configure_tax_remitter" | "add_vat_registration"; accountId: string; }) => void)`

### `onIdentityVerificationRequested`

The viewer pressed Verify on a `verify_identity` action the element can start. The element opens identity verification itself — this fires alongside, so a host can record the trip.

**Signature:** `((payload: { accountId: string; }) => void)`

### `onDepositRequested`

The viewer pressed Add money. The Wallet controller opens its deposit overlay.

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

Re-read the account. Call it when the viewer returns from verifying or depositing so the banners reflect the new state without waiting for the cache to expire.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<RequiredActionsElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                   | Targets                        |
| ----------------------- | ------------------------------ |
| `.whop-RequiredActions` | The outstanding-action banners |

```ts theme={null}
const wallet = whop.wallet.create({
  appearance: {
    classes: {
      'whop-RequiredActions': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

wallet.update({
  appearance: {
    classes: { 'whop-RequiredActions': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Wallet>`. Set it globally with `WhopElements({ appearance })`.
