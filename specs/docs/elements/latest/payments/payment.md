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

# PaymentElement

> Shows available payment methods and collects the selected method's required fields and disclosures. Use `change` to enable your pay button. In its activation handler, call `payments.createConfirmationToken()`. Confirm the token server-side, then pass any pending step to `payments.handleNextAction(…)`. Use `addressChange` for address-dependent updates.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Payments`](https://docs.whop.com/elements/latest/payments/overview). Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `select()`.

> [!NOTE]
>
> **Exclusive.**`PaymentElement` is an alternative to `CardElement` or `CardFields` in this Payments handle. Mount one at a time. Destroy it before mounting another.

> [!NOTE]
>
> **Waits for `BrandingElement`.** This element stays in its loading state until `BrandingElement` is mounted in the same Payments handle.

Mounts inside a `WhopPayments` scope. Renders the method tiles the charge offers, then whatever the selected method collects: the card fields, the fields it declares, or the Apple Pay button.

Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. It renders the method tiles for the charge and collects whatever the selected method declares, so a card, a wallet sheet and a bank redirect are the same one line.

```tsx React theme={null}
import { WhopElements, Payments, PaymentElement, BrandingElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Payments /* options */>
        <PaymentElement onChange={(e) => console.log(e)} onAddressChange={(e) => console.log(e)} />
        <BrandingElement />
      </Payments>
    </WhopElements>
  );
}
```

```tsx React Native theme={null}
import { useRef, useState } from 'react';
import { Button, ScrollView } from 'react-native';
import { EmailElement, PaymentElement, Payments, type PaymentsHandle } from '@whop/elements-react-native';

export function PaymentStep() {
  const payments = useRef<PaymentsHandle | null>(null);
  const [ready, setReady] = useState(false);

  return (
    <Payments
      ref={payments}
      accountId="biz_xxxxxxxx"
      plan="plan_xxxxxxxx"
      returnUrl="https://example.com/checkout/return"
    >
      <ScrollView>
        <EmailElement />
        <PaymentElement onChange={(p) => setReady(p.complete)} />
      </ScrollView>
      <Button
        title="Pay"
        disabled={!ready}
        onPress={async () => {
          const token = await payments.current!.createConfirmationToken();
          console.log(token.confirmationToken, token.type);
        }}
      />
    </Payments>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const payments = window.WhopElements().payments.create({ /* options */ });
  payments.create('payment', {
    onChange: (e) => console.log(e),
    onAddressChange: (e) => console.log(e)
  }).mount('#payments-payment');
  payments.create('branding').mount('#payments-branding');
</script>
```

```swift Swift theme={null}
import Elements
import SwiftUI

// .whopElements(environment:) runs once at the app root. See Getting started.
struct CheckoutScreen: View {
    @State private var selection = WhopPaymentSelection(isComplete: false, type: nil, displayName: nil, category: nil)

    var body: some View {
        WhopPayments(accountID: "biz_xxxx", charge: .plan(id: "plan_xxxx")) { payments in
            ScrollView {
                VStack(alignment: .leading, spacing: 20) {
                WhopPaymentElement(selection: $selection)
                    WhopBrandingElement()
                }
                .padding()
            }
        }
    }
}
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/payments/overview#playground).

[PaymentElement running on Android, in the React Native example app](https://app.revyl.ai/embed/c05028fb-0d33-4f36-b6f9-5339a4b9a054?controls=0)

## Props

**order** `string[]`

Controls display order in either layout. Listed types appear first in order. Unlisted types retain their relative order. Does not affect availability. Defaults to `[]`.

**wallets** `object`

Controls which wallets the element offers. Each key defaults to `'auto'`, which offers the wallet wherever the account, the buyer's device, and the charge support it. `'never'` removes that wallet from the method list. Use it when your page draws its own Apple Pay, Google Pay, or PayPal buttons with `paymentRequest`, so buyers see each wallet once. `payPal: 'never'` removes the PayPal method. Saved payment methods are unaffected. Defaults to `{}`. Since `v1.8.0`.

**properties**

**applePay** `"never" | "auto"`

**googlePay** `"never" | "auto"`

**payPal** `"never" | "auto"`

**fields** `object`

Controls billing-details collection. `billingDetails: 'minimal'` (default) follows each method's matrix. Methods collect name and the complete country format by default. An override may collect only the declared minimum or nothing. For cards, the minimum is name on card, country, and postal code. `'full'` requires name and the complete country format for every fresh method. `'never'` hides the block. `address: 'minimal'` collects only each method's declared minimum plus a country wherever that minimum lacks one (and the postal code too when `billingDetails: 'full'` says the checkout is taxed), except for wallets, crypto, bank debits, bank transfers, card-present and in-app purchase, which collect no address at all, and except for a method whose provider requires the complete billing address (`requires_full_billing_address`), whose tile follows the same rules as an unflagged checkout; otherwise it wins over `billingDetails: 'full'`; use it when the seller does not collect a full billing address. `phone: 'never'` hides only the billing phone input when your form already collects it; pass `billingDetails.phone` to `createConfirmationToken` for methods that require it. Pass the address to `createConfirmationToken` instead. The country selector includes only countries supported by the method and payment currency. With an installment plan selected, it narrows to the countries the plan serves — the billing country is the transaction country the charge processes under. It locks when only one is available. Defaults to `{"billingDetails":"minimal"}`.

**properties**

**billingDetails** `"minimal" | "full" | "never"`

**address** `"minimal" | "auto"`

**phone** `"never" | "auto"`

**defaultValues** `object`

Prefills the billing details when each method's billing block first renders. The buyer can edit every prefilled field. `address.country` is an ISO 3166-1 alpha-2 code and applies only when the selected method accepts it. `phone` fills the billing phone input for methods that collect one, in E.164 format such as `+14155552671`; a number outside the method's supported phone countries is ignored. Only fields the block renders are prefilled and sent. Pass anything else to `createConfirmationToken` in `billingDetails`. Since `v1.1.0`.

**properties**

**billingDetails** `object`

**properties**

**name** `string`

**phone** `string`

**address** `object`

**properties**

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**layout** `"accordion" | "horizontal"`

Picker layout. `accordion` (default) stacks methods and expands details inline. `horizontal` shows equal-width, non-scrolling tiles with selected details below. After four methods, a More tile opens a native selector. The method picked there occupies the final tile until the next selection. Selection and confirmation behave identically. Defaults to `"accordion"`.

**separated** `boolean`

`accordion` only. Adds spacing and separate card styling between methods. Ignored when `layout` is `horizontal`. Defaults to `false`.

**autoSelect** `boolean`

Whether to select the first offered method after resolution. The default `true` respects `order` and emits `selected` and `change` like a buyer interaction. It runs once before buyer interaction and does nothing when no method is offered. Set `false` to mount unselected. Independently, if an update removes the selected method, selection falls back to the first offered method. An already unselected element stays unselected. Defaults to `true`.

## Events

Pass callbacks in the create options or React props.

### `onChange`

Fires when selection changes. `complete: true` means the buyer selected a method and completed its card fields or required inputs. Use it to enable confirmation. `paymentMethod` is the selected payment method, saved or new, or `null` while nothing is selected: `displayName` and `icons` name the pick the way the element shows it (with `balance.imageUrl`, a balance owner's image), `card` carries a card's network, last four digits and funding, `requiresBillingPhone` says whether the method needs a phone in billing details, and `countries` lists where the method can pay from in the charge's currency. `type`, `requiresBillingPhone` and `method` at the top level are deprecated: read them from `paymentMethod`. `supportsBuyerFee` at the top level is deprecated too, with no replacement: Whop prices its buyer service fee itself.

**payload** `PaymentElementChangeEvent`

**properties**

**complete** `boolean` *required*

Whether the buyer selected a method and completed its card fields or required inputs. Use it to enable confirmation.

**paymentMethod** `PaymentElementPaymentMethod | null` *required*

The selected payment method, or `null` while nothing is selected.

**properties**

**id** `string | null` *required*

The saved payment method the buyer picked, or `null` for a method the buyer is entering now or a balance.

**type** `string` *required*

The payment method type: `card`, `klarna`, `apple_pay`, `balance`, and so on.

**category** `string` *required*

The family the type belongs to, such as `card`, `wallet`, `bnpl` or `balance`.

**displayName** `string` *required*

What to call the pick: a saved method's own label ("Visa •••• 4242", "Cash App \$tag"), a balance's name ("Acme", "Personal Balance"), a card's network and last four digits as the buyer types it, or else the method's own name ("Klarna").

**icons** `PaymentMethodIcons`

The pick's icon art: a saved method's own icons, a typed card's network art, or else the method's icons. Always an icon set, with SVG and PNG renditions.

**properties**

**square** `PaymentMethodIconVariants | null`

**properties**

**light** `object` *required*

**properties**

**svg** `string` *required*

**dark** `object` *required*

**properties**

**svg** `string` *required*

**card** `PaymentMethodIconVariants | null`

**properties**

**light** `object` *required*

**properties**

**svg** `string` *required*

**dark** `object` *required*

**properties**

**svg** `string` *required*

**balance** `object`

What is known about a balance pick beyond its type: `imageUrl` is the balance owner's image, a single raster image URL rather than an icon set, to show in place of `icons`. Absent for other methods, and for a balance whose owner has no image.

**properties**

**imageUrl** `string`

**card** `object`

What is known about a card pick: a saved card's details, or a typed card's as the buyer types. Absent for other methods.

**properties**

**network** `string`

**last4** `string`

**funding** `string`

**requiresBillingPhone** `boolean` *required*

Whether the method needs a phone in the billing details, including when `fields.phone` disables the element's own phone input.

**countries** `string[]` *required*

The lowercase two-letter country codes the method can pay from in the charge's currency.

**type** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `paymentMethod.type`.

**supportsBuyerFee** `boolean` *deprecated*

> [!WARNING]
>
> Deprecated. Whop prices its buyer service fee itself, so a page has no use for it. Removed in the next major.

**requiresBillingPhone** `boolean` *deprecated*

> [!WARNING]
>
> Deprecated. Use `paymentMethod.requiresBillingPhone`.

**method** `object` *deprecated*

> [!WARNING]
>
> Deprecated. Use `paymentMethod`. Absent when the buyer picks a saved method.

**properties**

**type** `string` *required*

**category** `string` *required*

**template** `string` *required*

**display\_name** `string` *required*

**countries** `object[]` *required*

**properties**

**country** `string` *required*

**min\_amount** `number | null` *required*

**max\_amount** `number | null` *required*

**min\_amount** `number | null` *required*

**max\_amount** `number | null` *required*

### `onAddressChange`

Fires about 300 ms after the internal billing address changes. Use it for tax, shipping, or other address-dependent updates. `country` is an ISO 3166-1 alpha-2 code. The payload omits other address keys when empty or unused for that country. `complete: true` means the billing block is valid and complete. This event fires only while the payment element owns address collection. With `fields.billingDetails: 'never'`, use your address source. With a mounted `AddressElement`, use its `change` event.

**payload** `object`

**properties**

**complete** `boolean` *required*

**address** `object` *required*

**properties**

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string` *required*

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

### `select`

Select a method through the same path as a buyer interaction. For example, `select('cashapp')` expands the tile and emits `selected` and `change`. Cards remain `complete: false` until their fields are complete. Unknown, unavailable, or amount-gated methods reject with code `METHOD_NOT_OFFERED`. `select(null)` clears selection and emits `change` with `complete: false`. Use with `autoSelect={false}` for full external control.

**Parameters**

**input** `string | null` *required*

**Returns** `Promise<void>`

### `mount`

Mounts the element in `target` and starts loading. React components mount themselves.

**Parameters**

**target** `string | HTMLElement` *required*

### `destroy`

Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

### `update`

Merges new props into the mounted element. In React, change the component props instead.

**Parameters**

**options** `Partial\<PaymentElementProps\>` *required*

## Styling

Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

| Class | Targets |
| - | - |
| `.whop-Address` | The address form root |
| `.whop-AddressErrorSummary` | The summary line shown when validation reveals missing or invalid fields |
| `.whop-AddressField` | One field cell in the address form |
| `.whop-AddressFieldError` | The error line for an address field: under it in the full layout, under the group for a composer message in the compact layout |
| `.whop-AddressFieldInput` | A text input in the address form |
| `.whop-AddressFieldInputInvalid` | Added to an address input while it fails validation |
| `.whop-AddressFieldInvalid` | Added to a compact field cell while it fails validation |
| `.whop-AddressFieldLabel` | Address field label: above the field, or inside it with floating labels |
| `.whop-AddressFieldSelect` | A select (country, state, organization type) in the address form |
| `.whop-AddressGroup` | The bordered group holding every field in the compact layout |
| `.whop-AddressLine2Toggle` | Collapsed address line 2 toggle |
| `.whop-AddressManualEntry` | The "Enter address manually" text button below the collapsed form — expands the full country format |
| `.whop-AddressSearchButton` | The search button at the trailing edge of the street field on a phone |
| `.whop-AddressSuggestion` | One suggestion row in the autocomplete overlay |
| `.whop-AddressSuggestionActive` | Added to the keyboard/pointer-active suggestion row |
| `.whop-AddressSuggestionManual` | The "Enter address manually" row closing the suggestions list |
| `.whop-AddressSuggestions` | The autocomplete suggestions overlay anchored to the address line 1 field |
| `.whop-AddressSuggestionsEmpty` | The quiet line shown when the query settled with no address matches |
| `.whop-CardField` | Card number, expiration, or security code field |
| `.whop-CardFieldError` | Card validation message |
| `.whop-CardFieldGroup` | The grouped card fields — number on top, expiration and security code below |
| `.whop-CardFieldInput` | Bordered PCI input container |
| `.whop-CardFieldInputFocused` | Focused PCI input container |
| `.whop-CardFieldInputInvalid` | Invalid or incomplete PCI input container |
| `.whop-CardLabel` | Card information label |
| `.whop-Payment` | The payment element root |
| `.whop-PaymentBalance` | One wallet in the balance tile |
| `.whop-PaymentBalanceHint` | The wallet's available amount in the charge currency |
| `.whop-PaymentBalanceList` | The balance tile's scrollable list of the buyer's wallets |
| `.whop-PaymentBalanceMore` | The sentinel row that pages in more balances as it scrolls into view |
| `.whop-PaymentBalanceRow` | The clickable balance row |
| `.whop-PaymentBalanceRowSelected` | Selected balance row |
| `.whop-PaymentBalanceRowUnavailable` | A wallet that cannot pay right now — blocked or empty — greyed and disabled |
| `.whop-PaymentBillingBlock` | The payment element's internal billing address block |
| `.whop-PaymentCardFields` | The inline card fields panel |
| `.whop-PaymentCompactBalance` | The selected balance on a compact direct checkout |
| `.whop-PaymentCompactSavedMethod` | The selected saved method on a compact direct checkout |
| `.whop-PaymentCurrencyFallback` | The switch to the fallback currency when nothing is offered |
| `.whop-PaymentDetailIcon` | The template icon beside the detail region's explainer |
| `.whop-PaymentDetailRegion` | The expanded detail region for a selected method — collection surfaces first (inline card fields, declared inputs), then the explainer and disclosure lines as the bottom block |
| `.whop-PaymentDetailSubtext` | The consent subtext under the explainer on wallet methods |
| `.whop-PaymentDetailText` | The detail region's explainer line |
| `.whop-PaymentError` | Unavailable payment methods error pane |
| `.whop-PaymentFieldError` | The Invalid message under a declared input |
| `.whop-PaymentFieldInput` | A declared method input in the detail region |
| `.whop-PaymentFieldInputInvalid` | Added to a declared input while its value fails the declared format |
| `.whop-PaymentFieldLabel` | Declared payment field label |
| `.whop-PaymentInstallmentDetail` | An installment option row's per-installment amount |
| `.whop-PaymentInstallmentLabel` | An installment option row's label |
| `.whop-PaymentInstallmentList` | The installment picker's option list |
| `.whop-PaymentInstallmentRadio` | The radio indicator on an installment option row |
| `.whop-PaymentInstallmentRow` | One installment option row |
| `.whop-PaymentInstallmentRowSelected` | The selected installment option row |
| `.whop-PaymentInstallments` | The payment method installment picker |
| `.whop-PaymentInstallmentsLabel` | The installment picker's heading |
| `.whop-PaymentInstallmentsNotice` | The issuer-fee disclaimer under a selected tier with no declared fee |
| `.whop-PaymentMandateLink` | The mandate terms link inside the mandate notice |
| `.whop-PaymentMandateNotice` | The mandate authorization notice on methods whose matrix configuration declares a mandate |
| `.whop-PaymentMethod` | One payment method — the row plus its expanding detail region |
| `.whop-PaymentMethodDetail` | The expanding region that reveals the selected method's detail — inline card fields, explainer, or declared inputs |
| `.whop-PaymentMethodIcon` | The method icon on a row |
| `.whop-PaymentMethodLabel` | The method display name on a row |
| `.whop-PaymentMethodMoreSelect` | The invisible native select stretched over the More tile |
| `.whop-PaymentMethodMoreTile` | Overflow tile with method icons and a native selector |
| `.whop-PaymentMethodPanel` | The selected method's detail panel below the tile row (horizontal layout) |
| `.whop-PaymentMethodRadio` | The radio indicator on a method row |
| `.whop-PaymentMethodRadioSelected` | Selected payment method radio |
| `.whop-PaymentMethodRow` | The clickable payment method row |
| `.whop-PaymentMethodRowSelected` | Selected payment method row |
| `.whop-PaymentMethods` | The payment method list |
| `.whop-PaymentMethodSelected` | Added to the selected payment method — its row plus its expanded detail region |
| `.whop-PaymentMethodSeparated` | Added to a method item, or a more-methods expander, while it renders as its own separated card |
| `.whop-PaymentMethodsSeparated` | Separated accordion method list |
| `.whop-PaymentMethodTile` | One method tile in the horizontal tile row |
| `.whop-PaymentMethodTileRow` | Horizontal method tile row with overflow methods in the final More tile |
| `.whop-PaymentMethodTileSelected` | Selected payment method tile |
| `.whop-PaymentMoreRow` | The "N more payment methods" expander row |
| `.whop-PaymentPayerDocument` | Currency-specific buyer identity document fields |
| `.whop-PaymentPayerDocumentError` | Buyer identity document error |
| `.whop-PaymentPayerDocumentLabel` | Buyer identity document label |
| `.whop-PaymentPayerDocumentType` | Buyer identity document type selector |
| `.whop-PaymentSavedMethod` | One saved payment method specifically — carries PaymentMethod too |
| `.whop-PaymentSavedMethodHint` | The saved method's trailing detail — a card's expiration |
| `.whop-PaymentSavedMethodRow` | The clickable saved method row specifically — carries PaymentMethodRow too |
| `.whop-PaymentSavedMethodRowSelected` | Selected saved payment method row |
| `.whop-PaymentSavedMethods` | The saved-methods list specifically — carries PaymentMethods too, so one rule styles both lists |
| `.whop-PaymentSavedMethodSeparated` | The separated marker on a saved method specifically |
| `.whop-PaymentSavedMore` | The control that fetches the next page of saved payment methods |
| `.whop-PaymentSavedMoreSpinner` | The spinner shown while the next page of saved payment methods loads |
| `.whop-PaymentSettlementNotice` | The settlement-window hint on methods whose matrix configuration declares one |

```ts theme={null}
const payments = whop.payments.create({
  appearance: {
    classes: {
      'whop-Address': { borderRadius: '8px', fontWeight: '600' },
      'whop-AddressErrorSummary': { borderRadius: '8px', fontWeight: '600' },
      'whop-AddressField': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

// 90 classes use this shape
payments.update({
  appearance: { classes: { 'whop-Address': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.

## Parameters

**order** `[WhopPaymentMethodType]`

Tile order for this mount, overriding the controller's `methodOrder`. Listed types take their list position; the rest keep their incoming order behind them.

**selection** `Binding\<WhopPaymentSelection\>?`

Reads the current selection and its completeness back out. The controller tracks both either way.

## `WhopPaymentSelection`

What a selection hands back:

* `isComplete: Bool`: the selected method has everything it needs
* `type: WhopPaymentMethodType?`: the selected method
* `displayName: String?`: its label, as the matrix spells it
* `category: String?`: `card`, `wallet`, `bank_debit`, …

## States

Shows skeleton tiles while the method matrix loads, and the failure message when the read fails. A method whose category this build cannot run is skipped rather than rendered as a broken tile. Selecting a wallet swaps the confirm surface for Apple Pay.

## Good to know

* Apple Pay needs nothing from your app: no merchant identifier, no `In-App Payments` capability. The merchant is the one registered on the Whop account, and the tile renders whenever the device can pay.
* A method that declares a `secure` field renders it as a hosted input. Those values never enter your process.
* A signed-in buyer's stored methods appear above the fresh ones. Mount [`WhopEmailElement`](https://docs.whop.com/elements/latest/payments/email#swift) to offer the sign-in that produces the credential; picking a stored row collects nothing and mints a reference.
* A card charge that publishes installment tiers shows a plan picker inside the card pane. A plan the buyer's card cannot take collapses rather than dimming, and a plan whose region does not pair with the billing country is refused before any tokenizer runs.
* A market that requires an identity document (an `ars` charge, today) renders the type picker and number field inside the pane. That number passes through your app on its way to the tokenizer, unlike the card, and still never reaches Whop.
* Card and secure-field values are tokenized before the mint, so the confirmation token is the only thing that crosses your app.
* Mount [`WhopAddressElement`](https://docs.whop.com/elements/latest/payments/address#swift) beside it when a method needs a billing country: the tile list narrows to the methods that country allows.

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

**order** `string[]`

Method display order, with Stripe's `paymentMethodOrder` semantics. Unlisted types keep their incoming relative order behind the listed ones. Also settable once on `<Payments>`.

**onChange** `(payload: PaymentChangePayload) =\> void`

Fires on every selection and completeness change. Gate your submit button on `complete`.

**style** `StyleProp\<ViewStyle\>`

Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.

**fallback** `ReactNode`

Rendered instead of the built-in skeleton while the element loads.

**onReady** `() =\> void`

Fires once the element is interactive. `<Payments>` groups these, so its own `onLoadingChange` is usually the one you want.

**onError** `(error: \{ message: string; code?: string \}) =\> void`

A load or configuration failure for this element. The element renders its own error face either way.

## `PaymentChangePayload`

What `onChange` hands back:

* `complete: boolean`: the selected method has everything it needs
* `type?: string`: the selected method type, e.g. `card`
* `method?: { type, category, template, display_name }`: the selected method's matrix entry

## States

Three skeleton rows until the method matrix lands, then the tiles. Selecting a tile expands its detail region beneath it. `onChange` reports `complete` when the selected method has everything it needs, which is what gates your submit button. A charge with no eligible method renders an explanatory empty state rather than nothing.

## Good to know

* Card numbers never pass through your code. The fields are PCI-isolated native inputs, and the SDK hands Whop a token, so your app stays out of PCI scope.
* Apple Pay and Google Pay both present the platform's own sheet. Neither needs anything from you: Apple Pay uses the merchant registered on the Whop account, so there is no merchant identifier to pass and no capability to add in Xcode, and `googlePayMerchantName` only sets the name shown in the sheet, which defaults to Whop.
* Redirect methods and 3D Secure open `ASWebAuthenticationSession` on iOS and Custom Tabs on Android. The system browser, never a WebView, so the issuer's page stays outside your app's trust boundary.
* Set `returnUrl` on `<Payments>` to an **https** URL you host. The API refuses anything but https or loopback (`PaymentsApi::ValidateReturnUrl`), so a custom app scheme is not available here. You do not register a deep link: after the issuer redirects, `handleNextAction` polls the payment to rest and closes the browser itself.

> [!NOTE]
>
> Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).


## Related topics

- [Payments](https://docs.whop.com/elements/latest/payments/overview.md)
- [Build a Checkout with Payment Elements](https://docs.whop.com/developer/guides/payment-elements.md)
- [CardElement](https://docs.whop.com/elements/latest/payments/card.md)
