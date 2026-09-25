> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# VerificationElement

> A banner asking the account holder to verify their identity, shown only while verification is outstanding — an account that has already verified renders nothing at all, so the element can sit permanently in a layout. The headline and status messages come from the API, with a shorter description when inviting the account holder to start verification, so they track the account's actual state: an unstarted account is invited to unlock cards and payouts, one under review reads as pending, and a failed or flagged one says so. Pressing the button reports `verificationRequested` and stays put, so the host mounts its own verification — the `verifications` controller's `kyc` element, say. Reads with the Dashboard handle's `accessToken`, which needs `payout:account:read`. A failed read renders nothing rather than an error — a nudge should never become the loudest thing on the page.

<Info>This page documents `@whop/elements@1.1.0` and `@whop/elements-react@1.1.0`.</Info>

*Since `v1.0.0`.*

Mounts inside [`Dashboard`](/elements/latest/dashboard/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="dashboard/verification">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Dashboard, VerificationElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Dashboard /* options */>
                <VerificationElement onVerificationRequested={(e) => console.log(e)} />
              </Dashboard>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const dashboard = window.WhopElements().dashboard.create({ /* options */ });
          dashboard.create('verification', { onVerificationRequested: (e) => console.log(e) }).mount('#dashboard-verification');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:dashboard/verification" data-whop-elements-version="1.1.0" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/latest/dashboard/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="kind" type="&#x22;individual&#x22; | &#x22;business&#x22;">
  Which verification the button starts. `individual` (KYC) is what unlocks payouts and a Whop card. `business` (KYB) covers that and additionally unlocks financing and business cards — use it for a company that will need those. Defaults to `"individual"`.
</ResponseField>

<ResponseField name="compact" type="boolean">
  Show a compact inline prompt without the supporting description. Defaults to `false`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onVerificationRequested`

The viewer pressed the verify button. The element never navigates — mount your own verification, such as the `verifications` controller, in answer to this.

**Signature:** `((payload: { accountId: string; }) => void)`

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

Re-read the account. Call it when the viewer returns from verifying so the banner reflects the new state without waiting for the cache to expire.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<VerificationElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                      | Targets                       |
| -------------------------- | ----------------------------- |
| `.whop-VerificationBanner` | The verification nudge banner |

```ts theme={null}
const dashboard = whop.dashboard.create({
  appearance: {
    classes: {
      'whop-VerificationBanner': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

dashboard.update({
  appearance: {
    classes: { 'whop-VerificationBanner': { fontWeight: '700' } }
  }
});
```

In React, pass `appearance` to `<Dashboard>`. Set it globally with `WhopElements({ appearance })`.
