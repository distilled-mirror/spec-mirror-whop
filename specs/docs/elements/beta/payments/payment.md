> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# PaymentElement

> Shows available payment methods and collects the selected method's required fields and disclosures. Use `change` to enable your pay button. In its activation handler, call `payments.createConfirmationToken()`. Confirm the token server-side, then pass any pending step to `payments.handleNextAction(…)`. Use `addressChange` for address-dependent updates.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

<div data-whop-platform="web">
  Mounts inside [`Payments`](/elements/beta/payments/overview). Pass props and callbacks through the create options or React props. Keep the created handle, or React `ref`, to call `select()`.

  <Note>**Exclusive.** `PaymentElement` is an alternative to `CardElement` or `CardFields` in this Payments handle. Mount one at a time. Destroy it before mounting another.</Note>
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  Mount inside `<Payments>`, which owns the charge and the confirmation token. `<Payments>` itself mounts inside `<WhopElements>`. It renders the method tiles for the charge and collects whatever the selected method declares, so a card, a wallet sheet and a bank redirect are the same one line.
</div>

<div data-whop-split style={{ display: "flex", gap: "1.5rem", alignItems: "flex-start", flexWrap: "wrap" }}>
  <div style={{ flex: "1 1 26rem", minWidth: 0 }}>
    <div data-whop-usage="payments/payment">
      <CodeGroup>
        ```tsx React theme={null}
        import { WhopElements, Payments, PaymentElement } from "@whop/elements-react";
        import { loadWhop } from "@whop/elements";

        function Example() {
          return (
            <WhopElements elements={loadWhop()}>
              <Payments /* options */>
                <PaymentElement onChange={(e) => console.log(e)} onAddressChange={(e) => console.log(e)} />
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
              applePayMerchantId="merchant.com.example"
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
        <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
        <script type="module">
          const payments = window.WhopElements().payments.create({ /* options */ });
          payments.create('payment', {
            onChange: (e) => console.log(e),
            onAddressChange: (e) => console.log(e)
          }).mount('#payments-payment');
        </script>
        ```
      </CodeGroup>
    </div>
  </div>

  <div style={{ flex: "1 1 20rem", minWidth: 0 }}>
    <div data-whop-platform="web">
      <div data-whop-demo-shell style={{ position: "relative", minHeight: "320px", transition: "min-height 200ms ease" }}>
        <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

        <div data-whop-demo-native="element:payments/payment" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
      </div>

      <p style={{ fontSize: "0.8125rem", opacity: 0.7 }}>Example data. [Open the Playground](/elements/beta/payments/overview#playground).</p>
    </div>

    <div data-whop-platform="react-native" style={{ display: "none" }}>
      <div style={{ width: "22rem", maxWidth: "100%" }}>
        <div data-whop-simulator-shell className="whop-ios-simulator" style={{ position: "relative", aspectRatio: "390 / 800", overflow: "hidden" }}>
          <iframe src={"https://app.revyl.ai/embed/c05028fb-0d33-4f36-b6f9-5339a4b9a054?controls=0"} title="PaymentElement running on Android, in the React Native example app" loading="lazy" allow="fullscreen; clipboard-read; clipboard-write" style={{ position: "absolute", inset: 0, width: "100%", height: "100%", border: 0, background: "transparent", display: "block" }} />
        </div>
      </div>
    </div>
  </div>
</div>

<div data-whop-platform="web">
  ## Props

  <ResponseField name="order" type="string[]">
    Controls display order in either layout. Listed types appear first in order. Unlisted types retain their relative order. Does not affect availability. Defaults to `[]`.
  </ResponseField>

  <ResponseField name="fields" type="{ billingDetails?: &#x22;full&#x22; | &#x22;minimal&#x22; | &#x22;never&#x22; | undefined; }">
    Controls billing-details collection. `billingDetails: 'minimal'` (default) follows each method's matrix. Methods collect name and the complete country format by default. An override may collect only the declared minimum or nothing. For cards, the minimum is name on card, country, and postal code. `'full'` requires name and the complete country format for every fresh method. `'never'` hides the block. Pass the address to `createConfirmationToken` instead. The country selector includes only countries supported by the method and payment currency. With an installment plan selected, it narrows to the countries the plan serves — the billing country is the transaction country the charge processes under. It locks when only one is available. Defaults to `{"billingDetails":"minimal"}`.
  </ResponseField>

  <ResponseField name="layout" type="&#x22;accordion&#x22; | &#x22;horizontal&#x22;">
    Picker layout. `accordion` (default) stacks methods and expands details inline. `horizontal` shows equal-width, non-scrolling tiles with selected details below. After four methods, a More tile opens a native selector. The method picked there occupies the final tile until the next selection. Selection and confirmation behave identically. Defaults to `"accordion"`.
  </ResponseField>

  <ResponseField name="separated" type="boolean">
    `accordion` only. Adds spacing and separate card styling between methods. Ignored when `layout` is `horizontal`. Defaults to `false`.
  </ResponseField>

  <ResponseField name="autoSelect" type="boolean">
    Whether to select the first offered method after resolution. The default `true` respects `order` and emits `selected` and `change` like a buyer interaction. It runs once before buyer interaction and does nothing when no method is offered. Set `false` to mount unselected. Independently, if an update removes the selected method, selection falls back to the first offered method. An already unselected element stays unselected. Defaults to `true`.
  </ResponseField>

  ## Events

  Pass callbacks in the create options or React props.

  ### `onChange`

  Fires when selection changes. `complete: true` means the buyer selected a method and completed its card fields or required inputs. Use it to enable confirmation. `method` provides category, per-currency countries, and amount bounds for dependent UI such as country fields. `supportsBuyerFee` indicates whether to include Whop's buyer service fee in the displayed total.

  **Signature:** `((payload: { complete: boolean; type?: string | undefined; supportsBuyerFee?: boolean | undefined; method?: { type: string; category: string; template: string; display_name: string; countries: ({ country: string; min_amount: number | null; max_amount: number | null; })[]; min_amount: number | null; max_amount: number | null; } | undefined; }) => void)`

  ### `onAddressChange`

  Fires about 300 ms after the internal billing address changes. Use it for tax, shipping, or other address-dependent updates. `country` is an ISO 3166-1 alpha-2 code. The payload omits other address keys when empty or unused for that country. `complete: true` means the billing block is valid and complete. This event fires only while the payment element owns address collection. With `fields.billingDetails: 'never'`, use your address source. With a mounted `AddressElement`, use its `change` event.

  **Signature:** `((payload: { complete: boolean; address: { line1?: string | undefined; line2?: string | undefined; city?: string | undefined; state?: string | undefined; postal_code?: string | undefined; country: string; }; }) => void)`

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

  ### `select`

  Select a method through the same path as a buyer interaction. For example, `select('cashapp')` expands the tile and emits `selected` and `change`. Cards remain `complete: false` until their fields are complete. Unknown, unavailable, or amount-gated methods reject with code `METHOD_NOT_OFFERED`. `select(null)` clears selection and emits `change` with `complete: false`. Use with `autoSelect={false}` for full external control.

  **Signature:** `(input: string | null) => Promise<void>`

  ### `mount`

  Mounts the element in `target` and starts loading. React components mount themselves.

  **Signature:** `(target: string | HTMLElement) => void`

  ### `destroy`

  Removes the element and releases its frame and subscriptions. You can call it more than once. React removes the element automatically.

  **Signature:** `() => void`

  ### `update`

  Merges new props into the mounted element. In React, change the component props instead.

  **Signature:** `(options: Partial<PaymentElementProps>) => void`

  ## Styling

  Style these parts through `appearance.classes`. Use camel case or kebab case for property names and include units. Page stylesheets can't reach the element's frame. The framework validates each declaration before injecting it.

  | Class                                 | Targets                                                                                                                                                                         |
  | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | `.whop-Address`                       | The address form root                                                                                                                                                           |
  | `.whop-AddressErrorSummary`           | The summary line shown when validation reveals missing or invalid fields                                                                                                        |
  | `.whop-AddressField`                  | One field cell in the address form                                                                                                                                              |
  | `.whop-AddressFieldError`             | The error line under an address field (full layout)                                                                                                                             |
  | `.whop-AddressFieldInput`             | A text input in the address form                                                                                                                                                |
  | `.whop-AddressFieldInputInvalid`      | Added to an address input while it fails validation                                                                                                                             |
  | `.whop-AddressFieldInvalid`           | Added to a compact field cell while it fails validation                                                                                                                         |
  | `.whop-AddressFieldLabel`             | Address field label in full layout                                                                                                                                              |
  | `.whop-AddressFieldSelect`            | A select (country, state, organization type) in the address form                                                                                                                |
  | `.whop-AddressLine2Toggle`            | Collapsed address line 2 toggle                                                                                                                                                 |
  | `.whop-AddressManualEntry`            | The "Enter address manually" text button below the collapsed form — expands the full country format                                                                             |
  | `.whop-AddressSuggestion`             | One suggestion row in the autocomplete overlay                                                                                                                                  |
  | `.whop-AddressSuggestionActive`       | Added to the keyboard/pointer-active suggestion row                                                                                                                             |
  | `.whop-AddressSuggestionManual`       | The "Enter address manually" row closing the suggestions list                                                                                                                   |
  | `.whop-AddressSuggestions`            | The autocomplete suggestions overlay anchored to the address line 1 field                                                                                                       |
  | `.whop-AddressSuggestionsEmpty`       | The quiet line shown when the query settled with no address matches                                                                                                             |
  | `.whop-CardField`                     | Card number, expiration, or security code field                                                                                                                                 |
  | `.whop-CardFieldError`                | Card validation message                                                                                                                                                         |
  | `.whop-CardFieldGroup`                | The grouped card fields — number on top, expiration and security code below                                                                                                     |
  | `.whop-CardFieldInput`                | Bordered PCI input container                                                                                                                                                    |
  | `.whop-CardFieldInputFocused`         | Focused PCI input container                                                                                                                                                     |
  | `.whop-CardFieldInputInvalid`         | Invalid or incomplete PCI input container                                                                                                                                       |
  | `.whop-CardLabel`                     | Card information label                                                                                                                                                          |
  | `.whop-Payment`                       | The payment element root                                                                                                                                                        |
  | `.whop-PaymentBalance`                | One wallet in the balance tile                                                                                                                                                  |
  | `.whop-PaymentBalanceHint`            | The wallet's available amount in the charge currency                                                                                                                            |
  | `.whop-PaymentBalanceList`            | The balance tile's scrollable list of the buyer's wallets                                                                                                                       |
  | `.whop-PaymentBalanceMore`            | The sentinel row that pages in more balances as it scrolls into view                                                                                                            |
  | `.whop-PaymentBalanceRow`             | The clickable balance row                                                                                                                                                       |
  | `.whop-PaymentBalanceRowSelected`     | Selected balance row                                                                                                                                                            |
  | `.whop-PaymentBalanceRowUnavailable`  | A wallet that cannot pay right now — blocked or empty — greyed and disabled                                                                                                     |
  | `.whop-PaymentBillingBlock`           | The payment element's internal billing address block                                                                                                                            |
  | `.whop-PaymentCardFields`             | The inline card fields panel                                                                                                                                                    |
  | `.whop-PaymentCompactBalance`         | The selected balance on a compact direct checkout                                                                                                                               |
  | `.whop-PaymentCompactSavedMethod`     | The selected saved method on a compact direct checkout                                                                                                                          |
  | `.whop-PaymentCurrencyFallback`       | The switch to the fallback currency when nothing is offered                                                                                                                     |
  | `.whop-PaymentDetailIcon`             | The template icon beside the detail region's explainer                                                                                                                          |
  | `.whop-PaymentDetailRegion`           | The expanded detail region for a selected method — collection surfaces first (inline card fields, declared inputs), then the explainer and disclosure lines as the bottom block |
  | `.whop-PaymentDetailSubtext`          | The consent subtext under the explainer on wallet methods                                                                                                                       |
  | `.whop-PaymentDetailText`             | The detail region's explainer line                                                                                                                                              |
  | `.whop-PaymentError`                  | Unavailable payment methods error pane                                                                                                                                          |
  | `.whop-PaymentFieldError`             | The Invalid message under a declared input                                                                                                                                      |
  | `.whop-PaymentFieldInput`             | A declared method input in the detail region                                                                                                                                    |
  | `.whop-PaymentFieldInputInvalid`      | Added to a declared input while its value fails the declared format                                                                                                             |
  | `.whop-PaymentFieldLabel`             | Declared payment field label                                                                                                                                                    |
  | `.whop-PaymentInstallmentDetail`      | An installment option row's per-installment amount                                                                                                                              |
  | `.whop-PaymentInstallmentLabel`       | An installment option row's label                                                                                                                                               |
  | `.whop-PaymentInstallmentList`        | The installment picker's option list                                                                                                                                            |
  | `.whop-PaymentInstallmentRadio`       | The radio indicator on an installment option row                                                                                                                                |
  | `.whop-PaymentInstallmentRow`         | One installment option row                                                                                                                                                      |
  | `.whop-PaymentInstallmentRowSelected` | The selected installment option row                                                                                                                                             |
  | `.whop-PaymentInstallments`           | The payment method installment picker                                                                                                                                           |
  | `.whop-PaymentInstallmentsLabel`      | The installment picker's heading                                                                                                                                                |
  | `.whop-PaymentInstallmentsNotice`     | The issuer-fee disclaimer under a selected tier with no declared fee                                                                                                            |
  | `.whop-PaymentMandateLink`            | The mandate terms link inside the mandate notice                                                                                                                                |
  | `.whop-PaymentMandateNotice`          | The mandate authorization notice on methods whose matrix configuration declares a mandate                                                                                       |
  | `.whop-PaymentMethod`                 | One payment method — the row plus its expanding detail region                                                                                                                   |
  | `.whop-PaymentMethodDetail`           | The expanding region that reveals the selected method's detail — inline card fields, explainer, or declared inputs                                                              |
  | `.whop-PaymentMethodIcon`             | The method icon on a row                                                                                                                                                        |
  | `.whop-PaymentMethodLabel`            | The method display name on a row                                                                                                                                                |
  | `.whop-PaymentMethodMoreSelect`       | The invisible native select stretched over the More tile                                                                                                                        |
  | `.whop-PaymentMethodMoreTile`         | Overflow tile with method icons and a native selector                                                                                                                           |
  | `.whop-PaymentMethodPanel`            | The selected method's detail panel below the tile row (horizontal layout)                                                                                                       |
  | `.whop-PaymentMethodRadio`            | The radio indicator on a method row                                                                                                                                             |
  | `.whop-PaymentMethodRadioSelected`    | Selected payment method radio                                                                                                                                                   |
  | `.whop-PaymentMethodRow`              | The clickable payment method row                                                                                                                                                |
  | `.whop-PaymentMethodRowSelected`      | Selected payment method row                                                                                                                                                     |
  | `.whop-PaymentMethods`                | The payment method list                                                                                                                                                         |
  | `.whop-PaymentMethodSeparated`        | Added to a method item while it renders as its own separated card                                                                                                               |
  | `.whop-PaymentMethodsSeparated`       | Separated accordion method list                                                                                                                                                 |
  | `.whop-PaymentMethodTile`             | One method tile in the horizontal tile row                                                                                                                                      |
  | `.whop-PaymentMethodTileRow`          | Horizontal method tile row with overflow methods in the final More tile                                                                                                         |
  | `.whop-PaymentMethodTileSelected`     | Selected payment method tile                                                                                                                                                    |
  | `.whop-PaymentMoreRow`                | The "N more payment methods" expander row                                                                                                                                       |
  | `.whop-PaymentPayerDocument`          | Currency-specific buyer identity document fields                                                                                                                                |
  | `.whop-PaymentPayerDocumentError`     | Buyer identity document error                                                                                                                                                   |
  | `.whop-PaymentPayerDocumentLabel`     | Buyer identity document label                                                                                                                                                   |
  | `.whop-PaymentPayerDocumentType`      | Buyer identity document type selector                                                                                                                                           |
  | `.whop-PaymentSavedMethod`            | One saved payment method specifically — carries PaymentMethod too                                                                                                               |
  | `.whop-PaymentSavedMethodHint`        | The saved method's trailing detail — a card's expiration                                                                                                                        |
  | `.whop-PaymentSavedMethodRow`         | The clickable saved method row specifically — carries PaymentMethodRow too                                                                                                      |
  | `.whop-PaymentSavedMethodRowSelected` | Selected saved payment method row                                                                                                                                               |
  | `.whop-PaymentSavedMethods`           | The saved-methods list specifically — carries PaymentMethods too, so one rule styles both lists                                                                                 |
  | `.whop-PaymentSavedMethodSeparated`   | The separated marker on a saved method specifically                                                                                                                             |
  | `.whop-PaymentSavedMore`              | The control that fetches the next page of saved payment methods                                                                                                                 |
  | `.whop-PaymentSavedMoreSpinner`       | The spinner shown while the next page of saved payment methods loads                                                                                                            |
  | `.whop-PaymentSaveNotice`             | The save-for-future-purchases consent line closing the detail region                                                                                                            |
  | `.whop-PaymentSettlementNotice`       | The settlement-window hint on methods whose matrix configuration declares one                                                                                                   |

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

  // 88 classes use this shape
  payments.update({
    appearance: { classes: { 'whop-Address': { fontWeight: '700' } } }
  });
  ```

  In React, pass `appearance` to `<Payments>`. Set it globally with `WhopElements({ appearance })`.
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  ## Props

  <ResponseField name="order" type="string[]">
    Method display order, with Stripe's `paymentMethodOrder` semantics. Unlisted types keep their incoming relative order behind the listed ones. Also settable once on `<Payments>`.
  </ResponseField>

  <ResponseField name="onChange" type="(payload: PaymentChangePayload) => void">
    Fires on every selection and completeness change. Gate your submit button on `complete`.
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

  ## `PaymentChangePayload`

  What `onChange` hands back:

  * `complete: boolean`: the selected method has everything it needs
  * `type?: string`: the selected method type, e.g. `card`
  * `method?: { type, category, template, display_name }`: the selected method's matrix entry

  ## States

  Three skeleton rows until the method matrix lands, then the tiles. Selecting a tile expands its detail region beneath it. `onChange` reports `complete` when the selected method has everything it needs, which is what gates your submit button. A charge with no eligible method renders an explanatory empty state rather than nothing.

  ## Good to know

  * Card numbers never pass through your code. The fields are PCI-isolated native inputs, and the SDK hands Whop a token, so your app stays out of PCI scope.
  * Apple Pay and Google Pay use the platform sheet through `PKPaymentAuthorizationController` and Google's `PaymentsClient`. Apple Pay needs a merchant identifier: it comes from the account's own Apple Pay registration, and `applePayMerchantId` on `<Payments>` overrides it, so the tile is hidden only when neither exists. Google Pay needs nothing from you; `googlePayMerchantName` only sets the name shown in the sheet, which defaults to Whop.
  * Redirect methods and 3D Secure open `ASWebAuthenticationSession` on iOS and Custom Tabs on Android. The system browser, never a WebView, so the issuer's page stays outside your app's trust boundary.
  * Set `returnUrl` on `<Payments>` to an **https** URL you host. The API refuses anything but https or loopback (`PaymentsApi::ValidateReturnUrl`), so a custom app scheme is not available here. You do not register a deep link: after the issuer redirects, `handleNextAction` polls the payment to rest and closes the browser itself.

  <Note>
    Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](/elements/beta/getting-started) and [Appearance](/elements/beta/appearance).
  </Note>
</div>
