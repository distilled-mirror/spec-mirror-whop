> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# BrandingElement

> Whop's merchant-of-record notice: the Whop wordmark with links to the buyer terms and privacy policy. Mount it alongside every payment collection surface.

<Info>This page documents `@whop/elements@1.0.0-beta.5` and `@whop/elements-react@1.0.0-beta.5`.</Info>

*Pre-release, not yet part of a stable release.*

<div data-whop-platform="web">
  Mounts inside [`Payments`](/elements/beta/payments/overview). Pass props and callbacks through the create options or React props.
</div>

<div data-whop-platform="swift" style={{ display: "none" }}>
  Mounts inside a `WhopPayments` scope. The merchant-of-record line every payment surface has to carry, with the Whop wordmark drawn as a path.
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. Required on every form that collects: `createConfirmationToken` refuses while none is mounted, and it themes with the rest of your form.
</div>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="payments/branding">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Payments, BrandingElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Payments /* options */>
                <BrandingElement />
              </Payments>
            </WhopElements>
          );
        }
        ```

        ```tsx React Native theme={null}
        import { BrandingElement, Payments } from '@whop/elements-react-native';

        <Payments accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx">
          {/* your form */}
          <BrandingElement />
        </Payments>
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const payments = window.WhopElements().payments.create({ /* options */ });
          payments.create('branding').mount('#payments-branding');
        </script>
        ```

        ```swift Swift theme={null}
        import Elements
        import SwiftUI

        // .whopElements(environment:) runs once at the app root. See Getting started.
        struct CheckoutScreen: View {

            var body: some View {
                WhopPayments(accountID: "biz_xxxx", charge: .plan(id: "plan_xxxx")) { payments in
                    ScrollView {
                        VStack(alignment: .leading, spacing: 20) {
                        WhopPaymentElement()
                            WhopBrandingElement()
                        }
                        .padding()
                    }
                }
            }
        }
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-platform="web">
      <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
        <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

        <div data-whop-demo-native="element:payments/branding" data-whop-elements-version="1.0.0-beta.5" style={{ position: "relative" }} />
      </div>

      <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/payments/overview#playground).</p>
    </div>

    <div data-whop-platform="react-native" style={{ display: "none" }}>
      <div style={{ width: "22rem", maxWidth: "100%" }}>
        <div data-whop-simulator-shell className="whop-ios-simulator" style={{ position: "relative", aspectRatio: "390 / 800", overflow: "hidden" }}>
          <iframe src={"https://app.revyl.ai/embed/c280bcb7-3262-4cc8-a0ef-d77bc0c49b55?controls=0"} title="BrandingElement running on Android, in the React Native example app" loading="lazy" allow="fullscreen; clipboard-read; clipboard-write" style={{ position: "absolute", inset: 0, width: "100%", height: "100%", border: 0, background: "transparent", display: "block" }} />
        </div>
      </div>
    </div>
  </div>
</div>

<div data-whop-platform="web">
  ## Props

  *This element takes no consumer props.*

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

  **Signature:** `(options: Partial<BrandingElementProps>) => void`

  ## Styling

  This element doesn't expose class names for styling. Use `appearance` (theme, accent color, variables) to restyle it.
</div>

<div data-whop-platform="swift" style={{ display: "none" }}>
  ## States

  Renders immediately. It fetches nothing and has no failure state.

  ## Good to know

  * **`createConfirmationToken` refuses while it is unmounted.** Whop is merchant of record on these sales, so the buyer has to be told so on the screen where they pay.
  * The wordmark is a vector path, not an asset, so the framework carries no image bundle.

  ## Install

  ```swift theme={null}
  dependencies: [
      .package(url: "https://github.com/whopio/elements-swift.git", from: "0.1.0")
  ]
  ```

  <Note>
    Mount it inside a `WhopPayments(accountID:charge:)` scope, which creates the controller and hands it to its content. `payments.buyer` is the signed-in buyer once an email sign-in has proven one. `WhopBrandingElement` has to be on screen too, because Whop is merchant of record on these sales and `createConfirmationToken` refuses without it. Style with `.whopElementsAppearance(_:)`. The module is `Elements`, not the wallet SDK's `WhopElements`. See [Getting started](/elements/beta/getting-started) and [Appearance](/elements/beta/appearance).
  </Note>
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  ## Props

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

  ## States

  Renders immediately. It has no data to load and no states of its own.

  ## Good to know

  * Required. `createConfirmationToken` refuses with a `BrandingRequiredError` while no `BrandingElement` is mounted, exactly as the web elements hold their reveal until branding attaches.
  * Mount it once anywhere inside `<Payments>`, typically under your submit button.
  * The wordmark follows the resolved theme's light or dark appearance on its own.

  <Note>
    Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](/elements/beta/getting-started) and [Appearance](/elements/beta/appearance).
  </Note>
</div>
