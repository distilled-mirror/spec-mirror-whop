> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## Agent Instructions
> Treat these docs and the Whop MCP server as the source of truth for Whop API behavior. Don't infer endpoint behavior from marketing pages.
> MCP server: connect https://mcp.whop.com/mcp (streamable HTTP, browser sign-in, no API key in the client config) to search these docs and read or change live Whop data. Setup: https://docs.whop.com/developer/guides/ai_and_mcp.md
> Whop CLI: install with `curl -fsSL https://whop.com/install.sh | sh` to scaffold, run, and deploy Whop apps and websites. It acts on production, not the sandbox. Overview: https://docs.whop.com/cli/overview.md Agent mode: https://docs.whop.com/cli/agent-mode.md
> Hosting: Whop serves a site or app from the build `whop apps deploy` uploads, with rollbacks, secrets, and server logs. Guide: https://docs.whop.com/developer/websites/hosting.md
> Plugins: in Claude Code, run `/plugin marketplace add whopio/plugins`, then `/plugin install whop@whop`. Guide: https://docs.whop.com/developer/guides/plugins/claude-code.md Every agent tool: https://docs.whop.com/developer/ai/overview.md
> Environments: build and test in the sandbox (API `https://sandbox-api.whop.com/api/v1`, keys at https://sandbox.whop.com/dashboard/developer, SDK environment `Sandbox`; no real money). Production (API `https://api.whop.com/api/v1`, keys at https://whop.com/dashboard/developer, SDK environment `Production`, the SDK default) moves real money, so use it only at launch. The two share no accounts, keys, products, or webhooks, and their keys look identical: never mix them. Sandbox guide: https://docs.whop.com/developer/guides/sandbox.md Comparison: https://docs.whop.com/developer/quickstart.md
> API: send `Authorization: Bearer <API_KEY>` from your server only, and pin a dated version with the `Api-Version-Date` header. OpenAPI spec: https://api.whop.com/api/v1/openapi.json
> Which API: build on the Current API, whose pages live under https://docs.whop.com/api-reference/ (start at https://docs.whop.com/api-reference/overview.md). Pages under /api-reference/legacy/ are the Legacy API. Use one only when https://docs.whop.com/api-reference/stability.md lists the resource as Legacy-only.

# EmailElement

> Collects the buyer's email and passes it to `payments.createConfirmationToken()` while mounted. Explicit `billingDetails.email` wins. A matching Whop account shows optional sign-in with code verification, or with the account's passkey where it has one. Successful sign-in unlocks saved payment methods in the payment element. Buyers can continue as guests.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Payments`](https://docs.whop.com/elements/latest/payments/overview). Pass props and callbacks through the create options or React props.

Mounts inside a `WhopPayments` scope. Collects the buyer's email, which every confirmation token requires, and the controller reads it for the mint whether or not you bind it.

Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. It collects the buyer's email and publishes it to the provider, so `createConfirmationToken` sends it without being passed anything.

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
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
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

Example data. [Open the Playground](https://docs.whop.com/elements/latest/payments/overview#playground).

[EmailElement running on Android, in the React Native example app](https://app.revyl.ai/embed/00ebe239-4c8b-4fe4-90d9-0e57771e2f35?controls=0)

## Props

**label** `boolean`

Shows the localized label. Set `false` when supplying your own. The input retains a localized `aria-label`. Defaults to `true`.

**placeholder** `string`

Placeholder text for the email input. Empty (default) renders `you@example.com`. With floating labels the label text is the placeholder instead. Defaults to `""`.

**labelMode** `"above" | "floating"`

`above` (default) shows the label above the input. `floating` uses the label text as the placeholder, then shows a small label above the typed text once the input has a value. Ignored when `label` is `false`. Defaults to `"above"`. Since `v1.5.0`.

**defaultValue** `string`

Seed value applied once at mount. Defaults to `""`.

## Events

Pass callbacks in the create options or React props.

### `onChange`

Fires when the email changes. `complete` is true when plausible. Sign-in fires it with the signed-in email, which also feeds `createConfirmationToken()`.

**payload** `object`

**properties**

**email** `string` *required*

**complete** `boolean` *required*

### `onLoaderStart`

Runs after the loading skeleton first paints and before `onReady`.

### `onReady`

Runs after the element's first complete paint.

### `onError`

Runs when the element fails to load or crashes. The fallback remains visible. Use `code` for programmatic handling. `sourceKey` identifies a failed host-state source.

**e** `object`

**properties**

**message** `string` *required*

**code** `string`

**sourceKey** `string`

## Methods

Call these on the handle returned by `create`, or through a React `ref`.

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<EmailElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-Email` | Email element root |
| `.whop-EmailChecking` | The running outline while the email is checked or a sign-in code is sent |
| `.whop-EmailCodeHint` | The line asking for the sign-in code |
| `.whop-EmailError` | Invalid email message |
| `.whop-EmailInput` | The email input |
| `.whop-EmailInputInvalid` | Email input with an implausible value |
| `.whop-EmailLabel` | Email field label |
| `.whop-EmailPasskey` | The passkey sign-in button inside the email input |
| `.whop-EmailSignedIn` | Signed-in buyer row |
| `.whop-EmailSignIn` | Welcome back sign-in control |
| `.whop-EmailSignInError` | Sign-in error message |
| `.whop-EmailSignOut` | Choose a different email control |
| `.whop-EmailWelcome` | The welcome face greeting over the account email |
| `.whop-VerificationError` | The failure message in the dialog |
| `.whop-VerificationInput` | The one-time-code input |
| `.whop-VerificationInputInvalid` | Added to the code input after a failed attempt |
| `.whop-VerificationInputSlot` | One digit box of the one-time-code input |
| `.whop-VerificationInputSlotActive` | Added to the code slot the buyer is typing into |
| `.whop-VerificationPending` | The verifying… line while a submit is in flight |

```ts theme={null}
const payments = whop.payments.create({
  appearance: {
    classes: {
      'whop-Email': { borderRadius: '8px', fontWeight: '600' },
      'whop-EmailChecking': { borderRadius: '8px', fontWeight: '600' },
      'whop-EmailCodeHint': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

// 19 classes use this shape
payments.update({
  appearance: { classes: { 'whop-Email': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.

## Parameters

**email** `Binding\<String\>?`

Reads the value back out. The controller collects it either way.

**showsLabel** `Bool`

Set false when you supply your own label; the input keeps its accessibility label. Defaults to `true`.

**placeholder** `String?`

Empty renders `you@example.com`.

## States

Renders immediately. An implausible address shows its error once the field loses focus, then live.

## Good to know

* Without it, pass `billingDetails.email` to `createConfirmationToken` yourself. The mint refuses a token with no email.
* An explicit `billingDetails.email` at mint time wins over what this element collected.
* A plausible address is probed against the account directory, debounced and again on blur. A recognized one offers a sign-in, and the element presents the six-digit code sheet itself.
* A verified sign-in unlocks the buyer's saved payment methods in [`WhopPaymentElement`](https://docs.whop.com/elements/latest/payments/payment#swift). Read `payments.buyer` for who they are, and `payments.signOut()` to forget them.
* The scoped token lasts an hour and does not refresh. When it lapses the saved lane comes back empty and the buyer pays with a fresh method, the same path a guest takes.

## Install

```swift theme={null}
dependencies: [
    .package(url: "https://github.com/whopio/elements-swift.git", from: "0.1.0")
]
```

> [!NOTE]
>
> Mount it inside a `WhopPayments(accountID:charge:)` scope, which creates the controller and hands it to its content. `payments.buyer` is the signed-in buyer once an email sign-in has proven one. `WhopBrandingElement` has to be on screen too, because Whop is merchant of record on these sales and `createConfirmationToken` refuses without it. Style with `.whopElementsAppearance(_:)`. The module is `Elements`, not the wallet SDK's `WhopElements`. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).

## Props

**defaultValue** `string`

Seed value applied once at mount.

**label** `boolean`

Shows the localized label. Defaults to `true`.

**placeholder** `string`

Empty renders `you@example.com`.

**onChange** `(payload: EmailChangePayload) =\> void`

Fires on every edit.

**style** `StyleProp\<ViewStyle\>`

Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.

**fallback** `ReactNode`

Rendered instead of the built-in skeleton while the element loads.

**onReady** `() =\> void`

Fires once the element is interactive. `<Payments>` groups these, so its own `onLoadingChange` is usually the one you want.

**onError** `(error: \{ message: string; code?: string \}) =\> void`

A load or configuration failure for this element. The element renders its own error face either way.

## `EmailChangePayload`

What `onChange` hands back:

* `email: string`: the current value
* `complete: boolean`: the address parses

## States

Renders immediately. `onChange` reports `complete` once the address parses, and the element shows its own validation message on blur.

## Good to know

* The published email is what the confirmation token carries as `billing_details.email`. Passing `billingDetails.email` to `createConfirmationToken` overrides it.
* Set `label={false}` when your form supplies its own label. The input keeps its accessibility label either way.

> [!NOTE]
>
> Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).


## Related topics

- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
- [Build a Checkout with Payment Elements](https://docs.whop.com/developer/guides/payment-elements.md)
- [BrandingElement](https://docs.whop.com/elements/latest/payments/branding.md)
