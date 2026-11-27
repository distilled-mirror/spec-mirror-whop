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

# CardElement

> Prearranged fields for card number, expiration, and security code. Create with `payments.create("card")`, enable your payment button from `onChange`, and confirm with `payments.createConfirmationToken()`. Card numbers remain in PCI-isolated hosted fields. `layout` supports `stacked` (default) and `compact`.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Payments`](https://docs.whop.com/elements/latest/payments/overview). Pass props and callbacks through the create options or React props.

> [!NOTE]
>
> **Exclusive.**`CardElement` is an alternative to `PaymentElement` or `CardFields` in this Payments handle. Mount one at a time. Destroy it before mounting another.

> [!NOTE]
>
> **Waits for `BrandingElement`.** This element stays in its loading state until `BrandingElement` is mounted in the same Payments handle.

Mounts inside a `WhopPayments` scope. The three card inputs as one unit, PCI-isolated by the tokenizer, so the number, expiry and security code never enter your process.

Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. Reach for it when the form takes a card and nothing else: it registers itself as the collection surface, so `createConfirmationToken` tokenizes it with nothing else wired.

```tsx React theme={null}
import { WhopElements, Payments, CardElement, BrandingElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Payments /* options */>
        <CardElement onChange={(e) => console.log(e)} />
        <BrandingElement />
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
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const payments = window.WhopElements().payments.create({ /* options */ });
  payments.create('card', { onChange: (e) => console.log(e) }).mount('#payments-card');
  payments.create('branding').mount('#payments-branding');
</script>
```

```swift Swift theme={null}
import Elements
import SwiftUI

// .whopElements(environment:) runs once at the app root. See Getting started.
struct CheckoutScreen: View {
    @State private var card = WhopCardState(isComplete: false, brand: nil)

    var body: some View {
        WhopPayments(accountID: "biz_xxxx", charge: .plan(id: "plan_xxxx")) { payments in
            ScrollView {
                VStack(alignment: .leading, spacing: 20) {
                WhopCardElement(layout: .stacked, state: $card)
                    WhopBrandingElement()
                }
                .padding()
            }
        }
    }
}
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/payments/overview#playground).

[CardElement running on Android, in the React Native example app](https://app.revyl.ai/embed/97dfb742-fe50-4fe7-a817-87ea4585f159?controls=0)

## Props

**layout** `"compact" | "stacked"`

`stacked` (default) places the card number above expiration and security code. `compact` places all three in one row. Defaults to `"stacked"`.

## Events

Pass callbacks in the create options or React props.

### `onChange`

Fires when card fields change. `complete` becomes true after the buyer fills all three. Use it to enable confirmation. `brand` is the detected card network. `funding` is the detected funding type (`credit`, `debit`, or `prepaid`), `null` until the number identifies one. `issuingCountry` is the lowercase two-letter code of the country the card was issued in, `null` until the number identifies one.

**payload** `object`

**properties**

**complete** `boolean` *required*

**brand** `string` *required*

**funding** `string | null` *required*

**issuingCountry** `string | null` *required*

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

**options** `Partial\<CardElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-Card` | Card element root |
| `.whop-CardError` | Card configuration error pane |
| `.whop-CardField` | Card number, expiration, or security code field |
| `.whop-CardFieldError` | Card validation message |
| `.whop-CardFieldGroup` | Stacked card number, expiration, and security code fields |
| `.whop-CardFieldInput` | Bordered PCI input container |
| `.whop-CardFieldInputFocused` | Focused PCI input container |
| `.whop-CardFieldInputInvalid` | Invalid or incomplete PCI input container |
| `.whop-CardFieldRow` | Compact card number, expiration, and security code row |

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

// 9 classes use this shape
payments.update({
  appearance: { classes: { 'whop-Card': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.

## Parameters

**layout** `WhopCardElement.Layout`

`.stacked` puts the number above expiry and security code; `.compact` puts all three in one row. Defaults to `.stacked`.

**publicKey** `String?`

An advanced publishable key. Omit it and the account's own is fetched.

**state** `Binding\<WhopCardState\>?`

Reads completeness and the detected brand back out.

## `WhopCardState`

What a selection hands back:

* `isComplete: Bool`: all three fields valid
* `brand: String?`: the detected network, once the number identifies one

## States

Renders immediately and validates as the buyer types. The brand appears once the number identifies one. `state.isComplete` turns true when all three fields are valid.

## Good to know

* Use it instead of [`WhopPaymentElement`](https://docs.whop.com/elements/latest/payments/payment#swift) when card is the only method you take. Mounting both is a conflict: the controller mints from one collecting surface.
* `createConfirmationToken(billingDetails:)` needs a cardholder name for the card lane, so pass one or mount an address element that collects it.
* `publicKey` is an advanced escape hatch. Omit it and the SDK fetches the account's tokenizer key.

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

**layout** `'stacked' | 'compact'`

`stacked` puts the number above expiry and security code. `compact` puts all three in one row. Defaults to `stacked`.

**onChange** `(payload: CardFieldsChangePayload) =\> void`

Fires on every edit with completeness and per-field errors.

**style** `StyleProp\<ViewStyle\>`

Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.

**fallback** `ReactNode`

Rendered instead of the built-in skeleton while the element loads.

**onReady** `() =\> void`

Fires once the element is interactive. `<Payments>` groups these, so its own `onLoadingChange` is usually the one you want.

**onError** `(error: \{ message: string; code?: string \}) =\> void`

A load or configuration failure for this element. The element renders its own error face either way.

## `CardFieldsChangePayload`

What `onChange` hands back:

* `complete: boolean`: every field is filled and valid
* `errors: Record<string, string>`: per-field messages, empty exactly when `complete` is true

## States

The three fields render together once the publishable key resolves. `onChange` reports `complete` when all three are valid, and the element shows its own per-field errors.

## Good to know

* Card numbers never pass through your code. The fields are PCI-isolated native inputs, and the SDK hands Whop a token, so your app stays out of PCI scope.
* Mounting this makes the provider's collection surface the card, so `createConfirmationToken` tokenizes it with no extra wiring. Do not mount it beside a `PaymentElement`: exactly one collection surface is live at a time.
* For a form where the three fields sit in your own layout, use [`CardFields`](https://docs.whop.com/elements/latest/payments/cardFields) instead.

> [!NOTE]
>
> Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).


## Related topics

- [CardFields](https://docs.whop.com/elements/latest/payments/cardFields.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
- [PaymentElement](https://docs.whop.com/elements/latest/payments/payment.md)
