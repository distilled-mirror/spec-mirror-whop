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

# ExpressCheckoutElement

> One-press Apple Pay, Google Pay and PayPal buttons for a checkout — the wallet collects whatever the session still needs (the buyer's email, a shipping address for physical goods, a phone number where the seller collects one in Apple Pay and Google Pay, and in Apple Pay a promo code where the seller offers entry) and one press finishes the purchase. It shares the checkout's entry slot with the full checkout element, so a handle mounts exactly one of the two: this button alone where the purchase is simple enough for a sheet to finish, or the full checkout surface for everything else. Renders only the wallets the buyer's device can actually pay with — and the checkout's own payment method configuration allows — using the served button art, and renders nothing where no wallet is available. PayPal appears where the seller accepts it, opens in its own window, and saves the buyer's PayPal account with the purchase, so a renewal, a free trial and later charges the seller makes work like a saved card. All three wallets require Apple Pay on the page's domain: first-party whop.com pages are pre-approved, and any other site must add Apple Pay to its domain through the [Domains API](/api-reference/beta/domains/domain) before any of these buttons renders there. Offering Google Pay on your own site is subject to the Google Pay API Terms of Service. A checkout the sheet cannot honestly finish — a waitlist join, a transfer, a form question only a page can ask — refuses loudly instead of rendering a doomed button.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Mounts inside [`Checkout`](https://docs.whop.com/elements/latest/checkout/overview). Pass props and callbacks through the create options or React props.

> [!NOTE]
>
> **Exclusive.**`ExpressCheckoutElement` is an alternative to `CheckoutElement` in this Checkout handle. Mount one at a time. Destroy it before mounting another.

Mount inside `<Checkout>`, which opens the checkout session from your `plan`, `items` or `checkoutConfiguration` and keeps its credential to itself. `<Checkout>` itself mounts inside `<WhopElements>`. One press on the button is the whole purchase: there is no confirmation token to hand to your server.

```tsx React theme={null}
import { WhopElements, Checkout, ExpressCheckoutElement } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Checkout /* options */>
        <ExpressCheckoutElement />
      </Checkout>
    </WhopElements>
  );
}
```

```tsx React Native theme={null}
import { Checkout, ExpressCheckoutElement } from '@whop/elements-react-native';

export function BuyNow({ onPurchased }: { onPurchased: (receiptId: string) => void }) {
  return (
    <Checkout
      plan="plan_xxxxxxxx"
      returnUrl="https://example.com/checkout/return"
      onComplete={({ receiptId }) => onPurchased(receiptId)}
    >
      <ExpressCheckoutElement onError={(e) => console.log(e.code, e.message)} />
    </Checkout>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const checkout = window.WhopElements().checkout.create({ /* options */ });
  checkout.create('expressCheckout').mount('#checkout-expressCheckout');
</script>
```

Example data. [Open the Playground](https://docs.whop.com/elements/latest/checkout/overview#playground).

[ExpressCheckoutElement running on Android, in the React Native example app](https://app.revyl.ai/embed/e16407ed-1c20-4475-ad16-f78b4669374f?controls=0)

## Props

**wallets** `object | ("apple_pay" | "google_pay" | "paypal_express")[]`

> [!WARNING]
>
> `wallets` as an array is deprecated and will be removed in the next major version. Pass `{ applePay, googlePay, payPal }` with `"auto"` or `"never"` instead.

Which wallets may render. Set `applePay`, `googlePay`, or `payPal` to `'never'` to leave that button out; each key defaults to `'auto'`, a filter over what the buyer’s device and the seller actually offer. The element never shows a wallet the device, the served matrix, or the checkout’s payment method configuration cannot back, and the render order stays best-native-first, with PayPal last. A list of wallet types (`['apple_pay', 'google_pay']`) is still accepted as an allowlist but is deprecated and will be removed in the next major version; an empty list renders nothing (warned in dev builds). Every wallet needs Apple Pay ready on the page’s domain through the [Domains API](https://docs.whop.com/api-reference/beta/domains/domain) — first-party whop.com pages are pre-approved; on any other site, add Apple Pay to the domain or the buttons stay hidden. Offering Google Pay on your own site is subject to the [Google Pay API Terms of Service](https://payments.developers.google.com/terms/sellertos). Defaults to `{}`.

**properties**

**applePay** `"auto" | "never"`

**googlePay** `"auto" | "never"`

**payPal** `"auto" | "never"`

**layout** `"auto" | "horizontal" | "vertical"`

How the buttons stack. `auto` follows the element’s own container — stacked in narrow containers, side by side in wide ones; `horizontal` and `vertical` force one arrangement. Defaults to `"auto"`.

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

**options** `Partial\<ExpressCheckoutElementProps\>` *required*

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
| `.whop-AddressSearchCancel` | The Cancel button beside the address search field |
| `.whop-AddressSearchInput` | The search field at the top of the full-screen address search |
| `.whop-AddressSuggestion` | One suggestion row in the autocomplete overlay |
| `.whop-AddressSuggestionActive` | Added to the keyboard/pointer-active suggestion row |
| `.whop-AddressSuggestionManual` | The "Enter address manually" row closing the suggestions list |
| `.whop-AddressSuggestions` | The autocomplete suggestions overlay anchored to the address line 1 field |
| `.whop-AddressSuggestionsEmpty` | The quiet line shown when the query settled with no address matches |
| `.whop-Card` | Card element root |
| `.whop-CardError` | Card configuration error pane |
| `.whop-CardField` | Card number, expiration, or security code field |
| `.whop-CardFieldError` | Card validation message |
| `.whop-CardFieldGroup` | The grouped card fields — number on top, expiration and security code below |
| `.whop-CardFieldInput` | Bordered PCI input container |
| `.whop-CardFieldInputFocused` | Focused PCI input container |
| `.whop-CardFieldInputInvalid` | Invalid or incomplete PCI input container |
| `.whop-CardFieldRow` | Compact card number, expiration, and security code row |
| `.whop-CardLabel` | Card information label |
| `.whop-CheckoutExpress` | The express checkout row — one-press wallet payments above the form |
| `.whop-CheckoutExpressApplePay` | The Apple Pay express button |
| `.whop-CheckoutExpressButtons` | The wallet button stack — vertical in a narrow container, side by side in a wide one |
| `.whop-CheckoutExpressComplete` | The standalone express button’s settled outcome line |
| `.whop-CheckoutExpressContinue` | The blocked-navigation press on the express row — finishes the trip to the payment’s off-site step |
| `.whop-CheckoutExpressDivider` | The divider between express checkout and the form |
| `.whop-CheckoutExpressError` | The retryable error line under the express button |
| `.whop-CheckoutExpressFinishPayment` | The finish-payment line shown while the payment still needs a step the buyer dismissed |
| `.whop-CheckoutExpressFinishPaymentButton` | The re-entry button that re-runs the payment’s pending step |
| `.whop-CheckoutExpressGooglePay` | The Google Pay express button |
| `.whop-CheckoutExpressPayPal` | The PayPal express button |
| `.whop-CheckoutExpressPhoneError` | The refusal line for a wallet-collected phone number the server could not text |
| `.whop-CheckoutExpressRetry` | The failed rest’s button — re-reads the reopened session so the buyer can pay again |
| `.whop-CheckoutExpressUnavailable` | The face shown when an express button cannot serve this checkout |
| `.whop-CompletePayment` | Completion surface root |
| `.whop-CompletePaymentAmount` | The amount the buyer must pay |
| `.whop-CompletePaymentBarcode` | The scannable barcode card |
| `.whop-CompletePaymentDocumentLink` | The hosted-instructions button |
| `.whop-CompletePaymentError` | Error pane |
| `.whop-CompletePaymentFrame` | Framed PSP step |
| `.whop-CompletePaymentHint` | Automatic-update hint |
| `.whop-CompletePaymentInstructions` | Payment-instructions pane |
| `.whop-CompletePaymentProviderMark` | The voucher network logo |
| `.whop-CompletePaymentQr` | The scannable QR card |
| `.whop-CompletePaymentReference` | A copyable code block (reference, payment key) |
| `.whop-CompletePaymentSettled` | Resting-state pane shown before closing |
| `.whop-CompletePaymentSettledFailed` | Failed resting-state pane shown before closing |
| `.whop-CompletePaymentSpinner` | Waiting spinner |
| `.whop-CompletePaymentTitle` | Dialog title |
| `.whop-CompletePaymentTransferDetails` | The bank-transfer detail rows |
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
| `.whop-TaxId` | Tax registration root |
| `.whop-TaxIdError` | API validation error |
| `.whop-TaxIdInput` | Registration value input |
| `.whop-TaxIdLabel` | Registration value label |
| `.whop-TaxIdTypeLabel` | Registration type label |
| `.whop-TaxIdTypeSelect` | Registration type selector |
| `.whop-Verification` | Verification-code dialog body |
| `.whop-VerificationDismiss` | Continue-as-guest control for unavailable sign-in |
| `.whop-VerificationError` | The failure message in the dialog |
| `.whop-VerificationExpired` | Expired sign-in pane |
| `.whop-VerificationHeading` | Code dialog heading and explanation |
| `.whop-VerificationHint` | Code-delivery explanation |
| `.whop-VerificationInput` | The one-time-code input |
| `.whop-VerificationInputInvalid` | Added to the code input after a failed attempt |
| `.whop-VerificationInputSlot` | One digit box of the one-time-code input |
| `.whop-VerificationInputSlotActive` | Added to the code slot the buyer is typing into |
| `.whop-VerificationPasskey` | Use-a-passkey-instead control |
| `.whop-VerificationPending` | The verifying… line while a submit is in flight |
| `.whop-VerificationResend` | Resend-code control with countdown |
| `.whop-VerificationRestart` | Expired-pane start-over control |
| `.whop-VerificationTitle` | Dialog header wordmark |

```ts theme={null}
const checkout = whop.checkout.create({
  appearance: {
    classes: {
      'whop-Address': { borderRadius: '8px', fontWeight: '600' },
      'whop-AddressErrorSummary': { borderRadius: '8px', fontWeight: '600' },
      'whop-AddressField': { borderRadius: '8px', fontWeight: '600' }
    }
  }
});

// 159 classes use this shape
checkout.update({
  appearance: { classes: { 'whop-Address': { fontWeight: '700' } } }
});
```

In React, pass `appearance` to `<Checkout>`. Set it globally with `WhopElements({ appearance })`.

## Props

**wallets** `('apple_pay' | 'google_pay')[]`

Which wallets may render, as a filter over what the device offers. The element never shows a wallet the device, the account or the checkout's payment method configuration cannot back. An empty list renders nothing. Defaults to both.

**style** `StyleProp\<ViewStyle\>`

Applied to the element's outer `View`. For theming, prefer `appearance.parts` on the provider, which covers every element on this surface. Note the React Native part names are their own set today, not the web's `whop-*` class names, so a web appearance object does not port across unchanged.

**fallback** `ReactNode`

Rendered instead of the built-in skeleton while the element loads.

**onReady** `() =\> void`

Fires once the element has settled: the button is up, or the element knows it renders nothing. `<Checkout>` waits for it, so its own `onLoadingChange` is usually the one you want.

**onError** `(error: \{ message: string; code?: string \}) =\> void`

A load or configuration failure for this element. The element renders its own error face either way.

## States

One skeleton pill while the session opens and the device is asked whether it can pay, then the platform's own Apple Pay or Google Pay button above the Powered by Whop notice. A device with no wallet, a wallet the checkout's payment method configuration disables, or a plan that is sold out or archived renders nothing. A checkout a sheet cannot honestly finish (a setup, a waitlist join, a transfer, a form question only a page can ask) renders an explanatory face and reports `EXPRESS_CHECKOUT_UNAVAILABLE` through `onError`. After the purchase the element shows its outcome: complete, processing, or a Finish payment or Try again press when the payment still needs the buyer.

## Good to know

* A device offers one wallet: Apple Pay on iOS, Google Pay on Android. That is why there is no `layout` prop here: there is never a second button to arrange.
* The sheet collects what the session still needs: the email when the session has none, a phone number when the seller collects one, and a shipping address for physical goods. A seller who verifies phone numbers gets a code step in a native modal after the sheet closes.
* On iOS the sheet reprices as the buyer changes card, so tax follows the billing address, and offers a promo code field when the seller allows codes. Google's Android sheet shows the checkout's total and has no code field.
* Your app is never navigated. `onComplete` on `<Checkout>` fires once when the purchase stands, with `sessionId` and `receiptId`, which is where you move the buyer on. Fulfill from webhooks, not from this callback.
* 3D Secure and other off-site steps open `ASWebAuthenticationSession` on iOS and Custom Tabs on Android, and the element polls the payment to rest. `returnUrl` on `<Checkout>` must be an **https** URL you host.
* Every `<Checkout>` option is create-time. Changing one after mount reports `CHECKOUT_OPTIONS_FROZEN` and keeps the original order; give `<Checkout>` a new `key` to sell something else.

> [!NOTE]
>
> Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Checkout>` around the element. `<Checkout>` takes `plan`, `items`, `checkoutConfiguration`, `quantity`, `promoCode`, `affiliateCode`, `attribution`, `metadata` and `returnUrl`, plus `onComplete`, `onError`, `onLoadingChange` and `fallback`. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md) and [Appearance](https://docs.whop.com/elements/latest/appearance.md).


## Related topics

- [CheckoutElement](https://docs.whop.com/elements/latest/checkout/checkout.md)
- [Add Express Checkout](https://docs.whop.com/developer/guides/express-checkout.md)
- [Checkout](https://docs.whop.com/elements/latest/checkout/overview.md)
