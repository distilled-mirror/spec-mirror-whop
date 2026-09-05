> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CardElement

> Prearranged fields for card number, expiration, and security code. Create with `payments.create("card")`, enable your payment button from `onChange`, and confirm with `payments.createConfirmationToken()`. Card numbers remain in PCI-isolated hosted fields. `layout` supports `stacked` (default) and `compact`.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

<div data-whop-platform="web">
  Mounts inside [`Payments`](/elements/beta/payments/overview). Pass props and callbacks through the create options or React props.

  <Note>**Exclusive.** `CardElement` is an alternative to `PaymentElement` or `CardFields` in this Payments handle. Mount one at a time. Destroy it before mounting another.</Note>
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. Reach for it when the form takes a card and nothing else: it registers itself as the collection surface, so `createConfirmationToken` tokenizes it with nothing else wired.
</div>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="payments/card">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Payments, CardElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Payments /* options */>
                <CardElement onChange={(e) => console.log(e)} />
              </Payments>
            </WhopElements>
          );
        }
        ```

        ```tsx React Native theme={null}
        import { CardElement, Payments } from '@whop/elements-react-native';

        export function CardStep() {
          return (
            <Payments accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx">
              <CardElement layout="stacked" onChange={({ complete }) => console.log(complete)} />
            </Payments>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const payments = window.WhopElements().payments.create({ /* options */ });
          payments.create('card', { onChange: (e) => console.log(e) }).mount('#payments-card');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-platform="web">
      <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
        <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

        <div data-whop-demo-native="element:payments/card" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
      </div>

      <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/payments/overview#playground).</p>
    </div>

    <div data-whop-platform="react-native" style={{ display: "none" }}>
      <div style={{ width: "22rem", maxWidth: "100%" }}>
        <div data-whop-simulator-shell className="whop-ios-simulator" style={{ position: "relative", aspectRatio: "390 / 800", overflow: "hidden" }}>
          <iframe src={"https://app.revyl.ai/embed/97dfb742-fe50-4fe7-a817-87ea4585f159?controls=0"} title="CardElement running on Android, in the React Native example app" loading="lazy" allow="fullscreen; clipboard-read; clipboard-write" style={{ position: "absolute", inset: 0, width: "100%", height: "100%", border: 0, background: "transparent", display: "block" }} />
        </div>
      </div>
    </div>
  </div>
</div>

<div data-whop-platform="web">
  ## Props

  <ResponseField name="layout" type="&#x22;compact&#x22; | &#x22;stacked&#x22;">
    `stacked` (default) places the card number above expiration and security code. `compact` places all three in one row. Defaults to `"stacked"`.
  </ResponseField>

  ## Events

  Pass callbacks in the create options or React props.

  ### `onChange`

  Fires when card fields change. `complete` becomes true after the buyer fills all three. Use it to enable confirmation. `brand` is the detected card network. `funding` is the detected funding type (`credit`, `debit`, or `prepaid`), `null` until the number identifies one. `issuingCountry` is the lowercase two-letter code of the country the card was issued in, `null` until the number identifies one.

  **Signature:** `((payload: { complete: boolean; brand: string; funding: string | null; issuingCountry: string | null; }) => void)`

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

  **Signature:** `(options: Partial<CardElementProps>) => void`

  ## Styling

  Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

  | Class                         | Targets                                                   |
  | ----------------------------- | --------------------------------------------------------- |
  | `.whop-Card`                  | Card element root                                         |
  | `.whop-CardError`             | Card configuration error pane                             |
  | `.whop-CardField`             | Card number, expiration, or security code field           |
  | `.whop-CardFieldError`        | Card validation message                                   |
  | `.whop-CardFieldGroup`        | Stacked card number, expiration, and security code fields |
  | `.whop-CardFieldInput`        | Bordered PCI input container                              |
  | `.whop-CardFieldInputFocused` | Focused PCI input container                               |
  | `.whop-CardFieldInputInvalid` | Invalid or incomplete PCI input container                 |
  | `.whop-CardFieldRow`          | Compact card number, expiration, and security code row    |
  | `.whop-CardSaveNotice`        | Payment method saving consent                             |

  ```ts theme={null}
  const payments = whop.payments.create({
    appearance: {
      classes: {
        'whop-Card': { borderRadius: '8px', fontWeight: '600' },
        'whop-CardError': { borderRadius: '8px', fontWeight: '600' },
        'whop-CardField': { borderRadius: '8px', fontWeight: '600' }
      }
    }
  });

  // 10 classes use this shape
  payments.update({
    appearance: { classes: { 'whop-Card': { fontWeight: '700' } } }
  });
  ```

  In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  ## Props

  <ResponseField name="layout" type="'stacked' | 'compact'">
    `stacked` puts the number above expiry and security code. `compact` puts all three in one row. Defaults to `stacked`.
  </ResponseField>

  <ResponseField name="onChange" type="(payload: CardFieldsChangePayload) => void">
    Fires on every edit with completeness and per-field errors.
  </ResponseField>

  <ResponseField name="style" type="StyleProp<ViewStyle>">
    Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.
  </ResponseField>

  <ResponseField name="fallback" type="ReactNode">
    Rendered instead of the built-in skeleton while the element loads.
  </ResponseField>

  <ResponseField name="onReady" type="() => void">
    Fires once the element is interactive. `<Payments>` groups these, so its own `onLoadingChange` is usually the one you want.
  </ResponseField>

  <ResponseField name="onError" type="(error: { message: string; code?: string }) => void">
    A load or configuration failure for this element. The element renders its own error face either way.
  </ResponseField>

  ## `CardFieldsChangePayload`

  What `onChange` hands back:

  * `complete: boolean`: every field is filled and valid
  * `errors: Record<string, string>`: per-field messages, empty exactly when `complete` is true

  ## States

  The three fields render together once the publishable key resolves. `onChange` reports `complete` when all three are valid, and the element shows its own per-field errors.

  ## Good to know

  * Card numbers never pass through your code. The fields are PCI-isolated native inputs, and the SDK hands Whop a token, so your app stays out of PCI scope.
  * Mounting this makes the provider's collection surface the card, so `createConfirmationToken` tokenizes it with no extra wiring. Do not mount it beside a `PaymentElement`: exactly one collection surface is live at a time.
  * For a form where the three fields sit in your own layout, use [`CardFields`](/elements/beta/payments/cardFields) instead.

  <Note>
    Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](/elements/beta/getting-started) and [Appearance](/elements/beta/appearance).
  </Note>
</div>
