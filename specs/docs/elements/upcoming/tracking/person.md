> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# PersonElement

> Everything the account knows about one person: who they are, where they came from, what they have spent, and every event they have performed, in order. The view a person click leads to: mount it on the page `links.person` points at, or present it over your own page when `people` or `events` raise `personOpened`. `identifier` takes any identifier a person has been seen under, so a `personOpened` payload resolves here unchanged.

Mounts inside [`Tracking`](/elements/upcoming/tracking/overview). `accountId` and `accessToken` come from there. Pass props and callbacks through the create options or React props.

<Note>You can mount this element **inline** (`create`) or open it as a **modal** overlay (`createOverlay`).</Note>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="tracking/person">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Tracking, PersonElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Tracking /* options */>
                <PersonElement />
              </Tracking>
            </WhopElements>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const tracking = window.WhopElements().tracking.create({ /* options */ });
          tracking.create('person').mount('#tracking-person');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
      <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

      <div data-whop-demo-native="element:tracking/person" data-whop-elements-version="" style={{ position: "relative" }} />
    </div>

    <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/tracking/overview#playground).</p>
  </div>
</div>

## Props

<ResponseField name="identifier" type="string">
  The person to show, as any identifier the account has seen them under: the stable `prsn_…` person ID, a Whop user ID, or an email. Required. `personOpened` carries two of these; prefer its `personId`, the stable key that names the same person on every surface. If nothing resolves, the view says so rather than rendering empty. Defaults to `""`.
</ResponseField>

## Events

Pass callbacks in the create options or React props.

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

**Signature:** `(options: Partial<PersonElementProps>) => void`

## Styling

This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.
