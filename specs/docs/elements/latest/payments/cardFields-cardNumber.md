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

# CardNumberElement

> PCI-isolated hosted card number field. Card numbers never reach the host page.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`CardFields`](https://docs.whop.com/elements/latest/payments/cardFields), in [`Payments`](https://docs.whop.com/elements/latest/payments/overview). Pass props and callbacks through the create options or React props.

> [!NOTE]
>
> **Waits for `BrandingElement`.** This element stays in its loading state until `BrandingElement` is mounted in the same Payments handle.

Goes inside a [`WhopCardFields`](https://docs.whop.com/elements/latest/payments/cardFields#swift) builder. The hosted card-number input, with the brand mark on its trailing edge.

Mounts inside [`CardFields`](https://docs.whop.com/elements/latest/payments/cardFields). The card number on its own, so your layout decides where it sits.

```tsx React theme={null}
import { WhopElements, Payments, CardFields, CardNumberElement, BrandingElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Payments /* options */>
        <CardFields>
          <CardNumberElement />
        </CardFields>
        <BrandingElement />
      </Payments>
    </WhopElements>
  );
}
```

```tsx React Native theme={null}
import { CardFields, CardNumberElement } from '@whop/elements-react-native';

<CardFields>
  <CardNumberElement />
</CardFields>
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const payments = window.WhopElements().payments.create({ /* options */ });
  const cardFields = payments.create('cardFields', { /* options */ });
  cardFields.create('cardNumber').mount('#payments-cardFields-cardNumber');
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
                WhopCardFields {
                    WhopCardNumberElement()
                    WhopCardExpiryElement()
                    WhopCardCVCElement()
                }
                    WhopBrandingElement()
                }
                .padding()
            }
        }
    }
}
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/payments/overview#playground).

[CardNumberElement running on Android, in the React Native example app](https://app.revyl.ai/embed/0c1e7b5a-e435-4f49-8595-481cf29490db?controls=0)

## Props

*This element takes no consumer props.*

## Events

Pass callbacks in the create options or React props.

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

**options** `Partial\<CardNumberElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-CardField` | Card number, expiration, or security code field |
| `.whop-CardFieldError` | Card validation message |
| `.whop-CardFieldInput` | Bordered PCI input container |
| `.whop-CardFieldInputFocused` | Focused PCI input container |
| `.whop-CardFieldInputInvalid` | Invalid or incomplete PCI input container |

```ts theme={null}
const payments = whop.payments.create({
  appearance: {
    classes: {
      'whop-CardField': { borderRadius: '8px', fontWeight: '600' },
      'whop-CardFieldError': { borderRadius: '8px', fontWeight: '600' },
      'whop-CardFieldInput': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

// 5 classes use this shape
payments.update({
  appearance: { classes: { 'whop-CardField': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.

## States

Renders immediately. The brand mark appears once the number identifies a network. An invalid number shows its error under the field.

## Good to know

* Outside a `WhopCardFields` builder it renders nothing: it reads the card unit that builder creates.

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

**style** `StyleProp\<ViewStyle\>`

Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.

## States

Renders as soon as its `CardFields` provider has a publishable key. Its validation message renders beneath it.

## Good to know

* Card numbers never pass through your code. The fields are PCI-isolated native inputs, and the SDK hands Whop a token, so your app stays out of PCI scope.
* Read completeness from the `CardFields` provider's `onChange` rather than per field.

> [!NOTE]
>
> Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).


## Related topics

- [CardFields](https://docs.whop.com/elements/latest/payments/cardFields.md)
- [CardElement](https://docs.whop.com/elements/latest/payments/card.md)
- [CardCvcElement](https://docs.whop.com/elements/latest/payments/cardFields-cardCvc.md)
