> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# RfiElement

> Everything the account still owes compliance, and the forms to answer it. Each row is one group of requirements — grouped by the system that asked, because each relays to its provider once its own items are answered — and pressing it opens that group’s form in place. Rows already with a reviewer are shown but not answerable. Renders an all-clear once nothing is outstanding, so it can sit permanently in a settings page.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

Mounts inside [`Verifications`](/elements/beta/verifications/overview). Pass props and callbacks through the create options or React props.

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="verifications/rfi">
      <CodeGroup>
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
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const verifications = window.WhopElements().verifications.create({ /* options */ });
          verifications.create('rfi', {
            onSubmitted: (e) => console.log(e),
            onCompleted: (e) => console.log(e),
            onLoadFailed: (e) => console.log(e)
          }).mount('#verifications-rfi');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:verifications/rfi" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/verifications/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="hideUnderReview" type="boolean">
  Hide the rows that are already with a reviewer, leaving only what the viewer can act on. Defaults to `false`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

### `onSubmitted`

One group of requirements was answered. Fires per group, not once per field.

**Signature:** `((payload: { source: "identity" | "payout" | "audit" | "card_issuing" | "bank" | "ads" | "application"; itemIds: string[]; }) => void)`

### `onCompleted`

Nothing is outstanding any more. Fires when the element first observes an empty list, including on mount.

**Signature:** `((payload: { accountId: string; }) => void)`

### `onLoadFailed`

The outstanding requests could not be read, or answering one failed.

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

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Signature:** `(target: string | HTMLElement) => void`

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

**Signature:** `() => void`

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Signature:** `(options: Partial<RfiElementProps>) => void`

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class              | Targets                                                               |
| ------------------ | --------------------------------------------------------------------- |
| `.whop-RfiRow`     | One request row: what is being asked, what it affects, and its action |
| `.whop-RfiSurface` | The outstanding-requests surface                                      |

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
