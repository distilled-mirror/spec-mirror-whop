> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# EmailElement

> Collects the buyer's email and passes it to `payments.createConfirmationToken()` while mounted. Explicit `billingDetails.email` wins. A matching Whop account shows optional sign-in with code verification. Successful sign-in unlocks saved payment methods in the payment element. Buyers can continue as guests.

<div data-whop-platform="web">
  Mounts inside [`Payments`](/elements/upcoming/payments/overview). Pass props and callbacks through the create options or React props.
</div>

<div data-whop-platform="swift" style={{ display: "none" }}>
  Mounts inside a `WhopPayments` scope. Collects the buyer's email, which every confirmation token requires, and the controller reads it for the mint whether or not you bind it.
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. It collects the buyer's email and publishes it to the provider, so `createConfirmationToken` sends it without being passed anything.
</div>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="payments/email">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Payments, EmailElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Payments /* options */>
                <EmailElement onChange={(e) => console.log(e)} />
              </Payments>
            </WhopElements>
          );
        }
        ```

        ```tsx React Native theme={null}
        import { EmailElement, Payments } from '@whop/elements-react-native';

        export function ContactStep() {
          return (
            <Payments accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx">
              <EmailElement onChange={({ email, complete }) => console.log(email, complete)} />
            </Payments>
          );
        }
        ```

        ```html JavaScript theme={null}
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const payments = window.WhopElements().payments.create({ /* options */ });
          payments.create('email', { onChange: (e) => console.log(e) }).mount('#payments-email');
        </script>
        ```

        ```swift Swift theme={null}
        import Elements
        import SwiftUI

        // .whopElements(environment:) runs once at the app root. See Getting started.
        struct CheckoutScreen: View {
            @State private var email = ""

            var body: some View {
                WhopPayments(accountID: "biz_xxxx", charge: .plan(id: "plan_xxxx")) { payments in
                    ScrollView {
                        VStack(alignment: .leading, spacing: 20) {
                        WhopEmailElement(email: $email)
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

        <div data-whop-demo-native="element:payments/email" data-whop-elements-version="" style={{ position: "relative" }} />
      </div>

      <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/upcoming/payments/overview#playground).</p>
    </div>

    <div data-whop-platform="react-native" style={{ display: "none" }}>
      <div style={{ width: "22rem", maxWidth: "100%" }}>
        <div data-whop-simulator-shell className="whop-ios-simulator" style={{ position: "relative", aspectRatio: "390 / 800", overflow: "hidden" }}>
          <iframe src={"https://app.revyl.ai/embed/00ebe239-4c8b-4fe4-90d9-0e57771e2f35?controls=0"} title="EmailElement running on Android, in the React Native example app" loading="lazy" allow="fullscreen; clipboard-read; clipboard-write" style={{ position: "absolute", inset: 0, width: "100%", height: "100%", border: 0, background: "transparent", display: "block" }} />
        </div>
      </div>
    </div>
  </div>
</div>

<div data-whop-platform="web">
  ## Props

  <ResponseField name="label" type="boolean">
    Shows the localized label. Set `false` when supplying your own. The input retains a localized `aria-label`. Defaults to `true`.
  </ResponseField>

  <ResponseField name="defaultValue" type="string">
    Seed value applied once at mount. Defaults to `""`.
  </ResponseField>

  <ResponseField name="placeholder" type="string">
    Placeholder text for the email input. Empty (default) renders `you@example.com`. Defaults to `""`.
  </ResponseField>

  ## Events

  Pass callbacks in the create options or React props.

  ### `onChange`

  Fires when the email changes. `complete` is true when plausible. Sign-in fires it with the signed-in email, which also feeds `createConfirmationToken()`.

  **Signature:** `((payload: { email: string; complete: boolean; }) => void)`

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

  **Signature:** `(options: Partial<EmailElementProps>) => void`

  ## Styling

  Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

  | Class                     | Targets                               |
  | ------------------------- | ------------------------------------- |
  | `.whop-Email`             | Email element root                    |
  | `.whop-EmailError`        | Invalid email message                 |
  | `.whop-EmailInput`        | The email input                       |
  | `.whop-EmailInputInvalid` | Email input with an implausible value |
  | `.whop-EmailLabel`        | Email field label                     |
  | `.whop-EmailSignedIn`     | Signed-in buyer row                   |
  | `.whop-EmailSignIn`       | Welcome back sign-in control          |
  | `.whop-EmailSignInError`  | Sign-in error message                 |
  | `.whop-EmailSignOut`      | Choose a different email control      |

  ```ts theme={null}
  const payments = whop.payments.create({
    appearance: {
      classes: {
        'whop-Email': { borderRadius: '8px', fontWeight: '600' },
        'whop-EmailError': { borderRadius: '8px', fontWeight: '600' },
        'whop-EmailInput': { borderRadius: '8px', fontWeight: '600' }
      }
    }
  });

  // 9 classes use this shape
  payments.update({
    appearance: { classes: { 'whop-Email': { fontWeight: '700' } } }
  });
  ```

  In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.
</div>

<div data-whop-platform="swift" style={{ display: "none" }}>
  ## Parameters

  <ResponseField name="email" type="Binding<String>?">
    Reads the value back out. The controller collects it either way.
  </ResponseField>

  <ResponseField name="showsLabel" type="Bool">
    Set false when you supply your own label; the input keeps its accessibility label. Defaults to `true`.
  </ResponseField>

  <ResponseField name="placeholder" type="String?">
    Empty renders `you@example.com`.
  </ResponseField>

  ## States

  Renders immediately. An implausible address shows its error once the field loses focus, then live.

  ## Good to know

  * Without it, pass `billingDetails.email` to `createConfirmationToken` yourself. The mint refuses a token with no email.
  * An explicit `billingDetails.email` at mint time wins over what this element collected.
  * A plausible address is probed against the account directory, debounced and again on blur. A recognized one offers a sign-in, and the element presents the six-digit code sheet itself.
  * A verified sign-in unlocks the buyer's saved payment methods in [`WhopPaymentElement`](/elements/upcoming/payments/payment#swift). Read `payments.buyer` for who they are, and `payments.signOut()` to forget them.
  * The scoped token lasts an hour and does not refresh. When it lapses the saved lane comes back empty and the buyer pays with a fresh method, the same path a guest takes.

  ## Install

  ```swift theme={null}
  dependencies: [
      .package(url: "https://github.com/whopio/elements-swift.git", from: "0.1.0")
  ]
  ```

  <Note>
    Mount it inside a `WhopPayments(accountID:charge:)` scope, which creates the controller and hands it to its content. `payments.buyer` is the signed-in buyer once an email sign-in has proven one. `WhopBrandingElement` has to be on screen too, because Whop is merchant of record on these sales and `createConfirmationToken` refuses without it. Style with `.whopElementsAppearance(_:)`. The module is `Elements`, not the wallet SDK's `WhopElements`. See [Getting started](/elements/upcoming/getting-started) and [Appearance](/elements/upcoming/appearance).
  </Note>
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  ## Props

  <ResponseField name="defaultValue" type="string">
    Seed value applied once at mount.
  </ResponseField>

  <ResponseField name="label" type="boolean">
    Shows the localized label. Defaults to `true`.
  </ResponseField>

  <ResponseField name="placeholder" type="string">
    Empty renders `you@example.com`.
  </ResponseField>

  <ResponseField name="onChange" type="(payload: EmailChangePayload) => void">
    Fires on every edit.
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

  ## `EmailChangePayload`

  What `onChange` hands back:

  * `email: string`: the current value
  * `complete: boolean`: the address parses

  ## States

  Renders immediately. `onChange` reports `complete` once the address parses, and the element shows its own validation message on blur.

  ## Good to know

  * The published email is what the confirmation token carries as `billing_details.email`. Passing `billingDetails.email` to `createConfirmationToken` overrides it.
  * Set `label={false}` when your form supplies its own label. The input keeps its accessibility label either way.

  <Note>
    Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](/elements/upcoming/getting-started) and [Appearance](/elements/upcoming/appearance).
  </Note>
</div>
