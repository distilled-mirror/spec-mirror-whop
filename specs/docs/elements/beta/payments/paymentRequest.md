> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# PaymentRequest

> Low-level Apple Pay or Google Pay sheet for custom buttons, express checkout, and shipping callbacks. Wallet tiles automate this flow through `payments.createConfirmationToken`. Await `canMakePayment()` to check availability and prime the sheet, then call `show(type)` synchronously in the user-interaction handler. The resolved `ctok` is a `ctok_`-prefixed confirmation token. Confirm it server-side like an element-minted token.

<Info>This page documents `@whop/elements@1.0.0` and `@whop/elements-react@1.0.0`.</Info>

*Since `v1.0.0`.*

<div data-whop-platform="web">
  Create this [`Payments`](/elements/beta/payments/overview) resource without mounting an element: `whop.payments.paymentRequest.create({ … })` in vanilla or `useWhop().payments.paymentRequest.create({ … })` in React.

  ## Options

  Pass these to `whop.payments.paymentRequest.create({ … })`.

  <ResponseField name="accountId" type="string" required>
    Account ID, prefixed `biz_`, that scopes every client-side call.
  </ResponseField>

  <ResponseField name="currency" type="string" required>
    Three-letter ISO 4217 payment currency code.
  </ResponseField>

  <ResponseField name="amount" type="number" required>
    Payment amount in minor units.
  </ResponseField>

  <ResponseField name="setupFutureUsage" type="&#x22;off_session&#x22; | &#x22;on_session&#x22;">
    Set only after displaying save consent. Marks the token for `off_session` or `on_session` use.
  </ResponseField>

  <ResponseField name="requestPayerEmail" type="boolean">
    Ask the sheet for the payer's email — the confirmation token requires one. Defaults to `true`.
  </ResponseField>

  <ResponseField name="requestPayerPhone" type="boolean">
    Ask the sheet for the payer's phone number, delivered as `payer.phone` on the result — in E.164 where it parses against the country of the contact that supplied it, as the wallet gave it otherwise; `payer.country` is the billing country. Google Pay collects it through the billing address the sheet already asks for. Defaults to `false`.
  </ResponseField>

  <ResponseField name="requestShipping" type="boolean">
    Ask the sheet for a shipping address (and offer `shippingOptions` when given). Defaults to `false`.
  </ResponseField>

  <ResponseField name="countryCode" type="string">
    Account ISO 3166-1 alpha-2 country code for the Apple Pay sheet. Omit it to use the account's published registration country. The sheet defaults to `US` when neither is available.
  </ResponseField>

  <ResponseField name="lineItems" type="PaymentRequestLineItem[]">
    Line items the sheet lists under the total.
  </ResponseField>

  <ResponseField name="shippingOptions" type="PaymentRequestShippingOption[]">
    Shipping options the sheet offers when `requestShipping` is set. Display-only — update `amount` from your change handler.
  </ResponseField>

  ## Methods

  Call these on the resource.

  <Warning>Call `show` synchronously during user activation. Await prerequisites first.</Warning>

  ### `canMakePayment`

  Returns availability for each wallet when the account offers it, the device supports it, and this page's origin is approved for the account (first-party whop.com pages are pre-approved; a merchant page needs its domain registered and verified as a [payment method domain](/api-reference/beta/payment-method-domains/payment-method-domain), and offering Google Pay there is subject to the [Google Pay API Terms of Service](https://payments.developers.google.com/terms/sellertos)). Apple Pay on non-WebKit browsers is the desktop iPhone-handoff flow, so mobile devices there report it unavailable — native Apple Pay on iOS browsers is unaffected. `order` ranks the wallets by which sheet is native to the browser — Apple Pay first on Safari, Google Pay first elsewhere — so express buttons can stack best-first. This primes `show()`. Always await it first.

  **Signature:** `() => Promise<WalletAvailability>`

  ### `show`

  Open the selected wallet sheet after awaiting `canMakePayment()`. Apple requires session creation within the gesture stack. Pass `email` when your page already collected one — the sheet takes it and never asks for a second. With `requestPayerEmail: false`, passing one is required, and the sheet refuses to open without it because confirmation tokens require an email. Call synchronously during user activation. Await prerequisites first because browsers revoke activation across asynchronous steps.

  **Signature:** `(type: "apple_pay" | "google_pay", provided?: { email?: string | undefined; } | undefined) => Promise<PaymentRequestResult>`

  ## Events

  Subscribe to events below. Each method returns an unsubscribe function.

  <Warning>Each event handler must call its documented reply method exactly once. Otherwise, the flow waits for the vendor timeout.</Warning>

  ### `onShippingAddressChange`

  The sheet's shipping address changed (redacted pre-authorization: city/state/postal/country only). Call `updateWith(…)` exactly once per event or the flow waits until the vendor times out. Returns the unsubscribe function.

  **Signature:** `(handler: (ev: ShippingAddressChangeEvent) => void) => () => void`

  ### `onShippingOptionChange`

  The buyer picked a shipping option. Amount-only contract: `updateWith` carries amount/lineItems/shippingOptions/errors. Call `updateWith(…)` exactly once per event or the flow waits until the vendor times out. Returns the unsubscribe function.

  **Signature:** `(handler: (ev: ShippingOptionChangeEvent) => void) => () => void`

  ### `onBillingAddressChange`

  Fires at sheet open and on card switches with the selected card's redacted billing address (city/state/postal/country only) — reprice the total for the address the charge taxes off. Answer `updateWith({ amount })` in minor units, or `updateWith({})` to keep the current total. Every event is answered exactly once: a slow, thrown, or superseded handler answers keep-current. Call `updateWith(…)` exactly once per event or the flow waits until the vendor times out. Returns the unsubscribe function.

  **Signature:** `(handler: (ev: BillingAddressChangeEvent) => void) => () => void`

  ### `onConfirmationToken`

  The sheet created a confirmation token. This is the same result that `show()` returns. Returns the unsubscribe function.

  **Signature:** `(handler: (ev: PaymentRequestResult) => void) => () => void`

  ### `onCancel`

  The buyer dismissed the sheet. Returns the unsubscribe function.

  **Signature:** `(handler: () => void) => () => void`
</div>

<div data-whop-platform="swift" style={{ display: "none" }}>
  No element and no controller: an object you drive yourself, for an express checkout button above the form. `canMakePayment()` primes the sheet, `show()` resolves to the same `ctok_` an element mints, and the sheet stays open until `complete(success:)` reports what your server said.

  ## Usage

  ```swift theme={null}
  import Elements
  import SwiftUI

  struct ExpressCheckout: View {
      @State private var request = WhopPaymentRequest(
          accountID: "biz_xxxx",
          charge: .plan(id: "plan_xxxx")
      )
      @State private var isAvailable = false

      var body: some View {
          VStack {
              if isAvailable {
                  WhopApplePayButton {
                      Task { await pay() }
                  }
                  .frame(height: 48)
              }
          }
          .task { isAvailable = await request.canMakePayment() }
      }

      private func pay() async {
          do {
              let token = try await request.show()
              let confirmed = try await myBackend.confirm(token.id)
              request.complete(success: confirmed)
          } catch {
              request.complete(success: false)
          }
      }
  }
  ```

  ## Parameters

  <ResponseField name="accountID" type="String" required>
    The account the charge belongs to, prefixed `biz_`.
  </ResponseField>

  <ResponseField name="configuration" type="WhopElementsConfiguration">
    `environment`, `returnURL` and `locale` behave as they do everywhere else. Defaults to `WhopElementsConfiguration()`.
  </ResponseField>

  <ResponseField name="charge" type="WhopCharge" required>
    `.plan(id:)` or `.amount(minorUnits:currency:)`. Settable later; the next `canMakePayment()` re-resolves it.
  </ResponseField>

  <ResponseField name="setupFutureUsage" type="WhopSetupFutureUsage?">
    `.offSession` or `.onSession`. Set only after showing save consent.
  </ResponseField>

  <ResponseField name="options" type="WhopPaymentRequest.Options">
    `requestPayerEmail` and `requestBillingAddress` ask the sheet for what you did not supply (both default to `true`), and `label` overrides the line item shown above the total.
  </ResponseField>

  ## `WhopConfirmationToken`

  What a selection hands back:

  * `id: String`: the `ctok_` to confirm server-side
  * `paymentMethodType: WhopPaymentMethodType`: `apple_pay` on this lane

  ## States

  `canMakePayment()` returns false when the device cannot pay, when the account has no Apple Pay merchant registered, or when the charge fails to resolve; `lastError` says which. `isPresenting` is true while the sheet is up. `show()` throws `walletUnavailable` rather than failing silently, and its message distinguishes a buyer who dismissed the sheet from a sheet that could not open at all. Retrying after a refused confirm is safe: a new `show()` supersedes the sheet the previous one left behind.

  ## Good to know

  * `show()` is a plain async call, so you can await whatever the charge needs before presenting the sheet.
  * Pair it with `WhopApplePayButton`, which is Apple's own `PKPaymentButton`. Apple's guidelines require the system control rather than a drawn imitation.
  * The sheet is asked for an email and a billing address exactly when you did not supply them, and its answers fill only the gaps.
  * The Apple Pay merchant is the one registered on the Whop account. Your app supplies nothing and needs no `In-App Payments` capability.

  ## Install

  ```swift theme={null}
  dependencies: [
      .package(url: "https://github.com/whopio/elements-swift.git", from: "0.1.0")
  ]
  ```

  <Note>
    `WhopPaymentRequest` needs no scope and no mounted element, so it is the one payments surface that does not require `WhopBrandingElement` on screen. Show the merchant-of-record line yourself. The module is `Elements`, not the wallet SDK's `WhopElements`. See [Getting started](/elements/beta/getting-started).
  </Note>
</div>
