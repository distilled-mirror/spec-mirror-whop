> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CapabilitiesElement

> The account's verification standing: whether individual and business verification are done, and which capabilities that unlocks. Read-only — pressing a verify button reports `verificationRequested` and stays put, so the host mounts its own flow, such as this namespace's `kyc` element. Both halves can be hidden, so a host that only wants the capability list, or only the two verification rows, can drop the other.

Mounts inside [`Verifications`](/elements/upcoming/verifications/overview). Pass props and callbacks through the create options or React props.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="verifications/capabilities">
      <CodeGroup>
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
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const verifications = window.WhopElements().verifications.create({ /* options */ });
          verifications.create('capabilities', { onVerificationRequested: (e) => console.log(e) }).mount('#verifications-capabilities');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:verifications/capabilities" data-whop-elements-version="" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/verifications/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="hideStatus" type="boolean">
  Hide the individual and business verification rows, leaving only the capabilities. Defaults to `false`.
</ResponseField>

<ResponseField name="hideCapabilities" type="boolean">
  Hide the capabilities list, leaving only the two verification rows. Defaults to `false`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onVerificationRequested`

The viewer pressed a verify button. The element never navigates — mount your own verification, such as this namespace’s `kyc` element, in answer to this.

**Signature:** `((payload: { accountId: string; kind: "individual" | "business"; }) => void)`

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

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<CapabilitiesElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class                             | Targets                                       |
| --------------------------------- | --------------------------------------------- |
| `.whop-CapabilityRow`             | One capability row: its name, hint and state  |
| `.whop-VerificationStatusRow`     | One verification row: its name and its action |
| `.whop-VerificationStatusSurface` | The verification status surface               |

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
