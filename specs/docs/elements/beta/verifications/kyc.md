> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# KycElement

> A complete identity-verification flow. It collects individual KYC or business KYB details, starts or resumes the hosted provider session, handles follow-up information and document requests, polls status, and renders the final result without requiring the host to build verification UI.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Verifications`](/elements/beta/verifications/overview). Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `refresh()` and `restart()`.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="verifications/kyc">
      <CodeGroup>
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
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
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
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:verifications/kyc" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/verifications/overview#playground).</p>
  </div>
</div>

## Props

*This element takes no consumer props.*

## Events

Pass callbacks in the create options or React props.

### `onStatusChanged`

The verification entered a new status, including the status found on first mount.

**Signature:** `((payload: { verificationId: string; status: "not_started" | "pending" | "processing" | "manual_review" | "approved" | "rejected" | "action_required"; }) => void)`

### `onCompleted`

Verification was approved. Fires once when the element first observes `approved`.

**Signature:** `((payload: { verificationId: string; }) => void)`

### `onActionRequired`

Whop needs follow-up information. The element renders and submits the requested fields itself.

**Signature:** `((payload: { verificationId: string; requestedInformation: ActionRequiredItem[]; }) => void)`

### `onLoadFailed`

The initial load, intake submission, follow-up submission, or an explicit restart failed.

**Signature:** `((payload: { reason: "access_denied" | "unavailable"; }) => void)`

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

Fetch the latest verification status immediately. Pending and review states also poll automatically.

**Signature:** `() => Promise<{ verificationId: string; status: "not_started" | "pending" | "processing" | "manual_review" | "approved" | "rejected" | "action_required"; } | null>`

### `restart`

Abandon the current attempt and start a fresh hosted session immediately.

**Signature:** `() => Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<KycElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class              | Targets                                                                              |
| ------------------ | ------------------------------------------------------------------------------------ |
| `.whop-KycFrame`   | The verification panel                                                               |
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
