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

# CardFields

> Three separately mountable, PCI-isolated card fields for custom layouts: number, expiration, and security code. Create with `payments.create("cardFields")`, place each field, enable your payment button from `onChange`, and confirm with `payments.createConfirmationToken()`. Card numbers remain in hosted fields.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Payments`](https://docs.whop.com/elements/latest/payments/overview). Create it to get a handle, then mount its elements on that handle. Call `destroy()` to remove the sub-controller and free its slot. Create it again to get a fresh handle.

> [!NOTE]
>
> **Exclusive.**`CardFields` is an alternative to `PaymentElement` or `CardElement` in this Payments handle. Mount one at a time. Destroy it before mounting another.

## Preview

A live, interactive demo of this sub-controller's default arrangement with example data:

Mounts inside a `WhopPayments` scope. The same three hosted inputs as [`WhopCardElement`](https://docs.whop.com/elements/latest/payments/card#swift), placed one by one inside its content builder so your own layout decides where each sits.

The provider for individually placed card fields. Wrap the part of your form that holds them, then put `CardNumberElement`, `CardExpiryElement` and `CardCvcElement` wherever your layout wants them.

## Preview

Running in the React Native example app:

[CardFields running on Android, in the React Native example app](https://app.revyl.ai/embed/4a22f507-3667-4162-b83f-1d7a7d4fa900?controls=0)

```tsx React theme={null}
import { WhopElements, Payments, CardFields, CardNumberElement, CardExpiryElement, CardCvcElement, BrandingElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Payments /* options */>
        <CardFields>
          <CardNumberElement />
          <CardExpiryElement />
          <CardCvcElement />
        </CardFields>
        <BrandingElement />
      </Payments>
    </WhopElements>
  );
}
```

```tsx React Native theme={null}
import { View } from 'react-native';
import {
  CardCvcElement,
  CardExpiryElement,
  CardFields,
  CardNumberElement,
  Payments,
} from '@whop/elements-react-native';

export function CardForm() {
  return (
    <Payments accountId="biz_xxxxxxxx" plan="plan_xxxxxxxx">
      <CardFields onChange={({ complete }) => console.log(complete)}>
        <CardNumberElement />
        <View style={{ flexDirection: 'row', gap: 8 }}>
          <CardExpiryElement style={{ flex: 1 }} />
          <CardCvcElement style={{ flex: 1 }} />
        </View>
      </CardFields>
    </Payments>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const payments = window.WhopElements().payments.create({ /* options */ });
  const cardFields = payments.create('cardFields', { /* options */ });
  cardFields.create('cardNumber').mount('#payments-cardFields-cardNumber');
  cardFields.create('cardExpiry').mount('#payments-cardFields-cardExpiry');
  cardFields.create('cardCvc').mount('#payments-cardFields-cardCvc');
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
                WhopCardFields(state: $card) {
                    WhopCardNumberElement()
                    HStack(alignment: .top, spacing: 8) {
                        WhopCardExpiryElement()
                        WhopCardCVCElement()
                    }
                }
                    WhopBrandingElement()
                }
                .padding()
            }
        }
    }
}
```

## Options

Pass these to `payments.create('cardFields', { … })`, or as props on `<CardFields>` in React. Parent-injected props never appear here.

**networks** `CardNetworkArt[]`

The seller's accepted card networks in display order — the matrix card entry's `networks` objects, whose API-served icons drive the number field's brand art. Omit it to fetch alongside the key.

**properties**

**brand** `string` *required*

One of `"visa"`, `"mastercard"`, `"amex"`, `"discover"`, `"jcb"`, `"diners"`, `"unionpay"`, `"elo"`, or `"maestro"`.

**icons** `object` *required*

**properties**

**square** `object` *required*

**properties**

**light** `object` *required*

**properties**

**svg** `string` *required*

**dark** `object` *required*

**properties**

**svg** `string` *required*

**layout** `"compact" | "stacked"`

`stacked` (default) shapes composed fields as a number row above expiration and security code. `compact` shapes one row. Separately mounted fields ignore this layout and render with full borders and rounded corners. Defaults to `"stacked"`.

**publicKey** `string`

Advanced Basis Theory publishable key. Omit it to fetch the key automatically.

## Events

Pass callbacks in the create options or React props.

### `onChange`

Fires when completeness changes. `complete` becomes true after the buyer fills all three fields. Use it to enable confirmation. `brand` is the detected card network as the field reports it; `network` is the same detection in the API's network vocabulary (matches the matrix card entry's `networks`, `null` while unknown). `last4` is the number's last four digits, `null` until the number is far enough along. `funding` is the detected funding type (`credit`, `debit`, or `prepaid`), `null` until the number identifies one. `issuingCountry` is the lowercase two-letter code of the country the card was issued in, `null` until the number identifies one.

**payload** `object`

**properties**

**complete** `boolean` *required*

**brand** `string` *required*

**network** `string | null` *required*

One of `"visa"`, `"mastercard"`, `"amex"`, `"discover"`, `"jcb"`, `"diners"`, `"unionpay"`, `"elo"`, `"maestro"`, or `null`.

**last4** `string | null` *required*

**funding** `string | null` *required*

**issuingCountry** `string | null` *required*

## Methods

Call these on the sub handle from `payments.create('cardFields', { … })`.

### `tokenize`

Advanced method that tokenizes the three fields without creating a confirmation token. It emits `tokenized` to the payments controller. Validation errors appear on the failing field, and the method throws. Use `payments.createConfirmationToken()` to confirm a payment.

**Parameters**

**input** `object` *required*

**properties**

**accountId** `string`

**Returns** `Promise<object>`

**token** `string` *required*

### `collect`

`payments.createConfirmationToken()` calls this action for `cardFields`. Don't call it directly. It tokenizes the fields and returns the data used to create the confirmation token.

**Parameters**

**input** `object` *required*

**properties**

**billingDetails** `object`

**properties**

**email** `string`

**name** `string`

**address** `object`

**properties**

**country** `string`

**line1** `string`

**city** `string`

**postal\_code** `string`

**Returns** `Promise<object>`

**paymentMethod** `object` *required*

**properties**

**type** `string` *required*

**category** `string` *required*

**card** `object` *required*

**properties**

**token** `string` *required*

**billingDetails** `object` *required*

**properties**

**email** `string` *required*

**name** `string`

**address** `object`

**properties**

**country** `string`

**line1** `string`

**city** `string`

**postal\_code** `string`

### `update`

Merges new props and callbacks into the sub-controller.

**Parameters**

**options** `Partial\<CardFieldsSubOptions\>` *required*

### `destroy`

Destroys the sub-controller and its elements, then frees its exclusive slot. A later `create("cardFields")` starts fresh.

## Parameters

**publicKey** `String?`

An advanced publishable key. Omit it and the account's own is fetched.

**layout** `WhopCardElement.Layout`

Only read when a composing unit arranges the fields itself. Separately mounted fields ignore it.

**state** `Binding\<WhopCardState\>?`

Reads completeness and the detected brand back out.

**content** `() -\> Content` *required*

Where the three field views go. Anything else in here renders normally.

## `WhopCardState`

What a selection hands back:

* `isComplete: Bool`: all three fields valid
* `brand: String?`: the detected network

## States

Each field renders immediately and validates as the buyer types. `state.isComplete` turns true when all three are valid, wherever you put them.

## Good to know

* The three field views only work inside a `WhopCardFields` builder: they read the unit it creates. Mounted anywhere else they render nothing.
* Mount all three. A unit missing one never completes, so the confirm button never enables.

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

**children** `ReactNode` *required*

Your layout, with the three field elements somewhere inside it.

**publicKey** `string`

An Advanced tokenizer publishable key. Omit it and the SDK fetches the account's key itself.

**layout** `'stacked' | 'compact'`

Shapes fields that are composed together. Separately mounted fields ignore it and render with full borders. Defaults to `stacked`.

**onChange** `(payload: CardFieldsChangePayload) =\> void`

Fires on every edit with the combined state of the mounted fields.

## `CardFieldsChangePayload`

What `onChange` hands back:

* `complete: boolean`: every mounted field is filled and valid
* `errors: Record<string, string>`: per-field messages, keyed `cardNumber`, `cardExpiry`, `cardCvc`

## States

Its children render once the publishable key resolves. `onChange` reports the combined state of every mounted field, so one handler covers the whole card.

## Good to know

* Card numbers never pass through your code. The fields are PCI-isolated native inputs, and the SDK hands Whop a token, so your app stays out of PCI scope.
* Every field must be inside the same `CardFields`. They share one tokenization, so a field mounted outside it collects nothing.
* `CardFields` registers the collection surface, so `createConfirmationToken` tokenizes the card with no extra wiring.
* Fetching the publishable key is automatic. Pass `publicKey` only if you already hold one.

> [!NOTE]
>
> Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).

## Elements

The elements this sub-controller mounts. Each has its own page:

> [card] [CardNumberElement](https://docs.whop.com/elements/latest/payments/cardFields-cardNumber.md)
>
> PCI-isolated hosted card number field. Card numbers never reach the host page.

> [card] [CardExpiryElement](https://docs.whop.com/elements/latest/payments/cardFields-cardExpiry.md)
>
> PCI-isolated hosted card expiration field.

> [card] [CardCvcElement](https://docs.whop.com/elements/latest/payments/cardFields-cardCvc.md)
>
> PCI-isolated hosted card security code field.


## Related topics

- [CardExpiryElement](https://docs.whop.com/elements/latest/payments/cardFields-cardExpiry.md)
- [CardNumberElement](https://docs.whop.com/elements/latest/payments/cardFields-cardNumber.md)
- [CardCvcElement](https://docs.whop.com/elements/latest/payments/cardFields-cardCvc.md)
