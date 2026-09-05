> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CardFields

> Three separately mountable, PCI-isolated card fields for custom layouts: number, expiration, and security code. Create with `payments.create("cardFields")`, place each field, enable your payment button from `onChange`, and confirm with `payments.createConfirmationToken()`. Card numbers remain in hosted fields.

<Info>This page documents `@whop/elements@1.0.0-beta.3` and `@whop/elements-react@1.0.0-beta.3`.</Info>

*Pre-release, not yet part of a stable release.*

<div data-whop-platform="web">
  Mounts inside [`Payments`](/elements/beta/payments/overview). Create it to get a handle, then mount its elements on that handle. Call `destroy()` to remove the sub-controller and free its slot. Create it again to get a fresh handle.

  <Note>**Exclusive.** `CardFields` is an alternative to `PaymentElement` or `CardElement` in this Payments handle. Mount one at a time. Destroy it before mounting another.</Note>

  ## Preview

  A live, interactive demo of this sub-controller's default arrangement with example data:

  <div data-whop-demo-shell style={{ position: "relative", minHeight: "480px", transition: "min-height 200ms ease" }}>
    <div data-whop-demo-skeleton style={{ position: "absolute", inset: "0", borderRadius: "12px", background: "rgba(140, 140, 140, 0.12)", pointerEvents: "none", transition: "opacity 200ms ease" }} />

    <div data-whop-demo-native="unit:card-fields" data-whop-elements-version="1.0.0-beta.3" style={{ position: "relative" }} />
  </div>
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  The provider for individually placed card fields. Wrap the part of your form that holds them, then put `CardNumberElement`, `CardExpiryElement` and `CardCvcElement` wherever your layout wants them.

  ## Preview

  Running in the React Native example app:

  <div style={{ width: "22rem", maxWidth: "100%" }}>
    <div data-whop-simulator-shell className="whop-ios-simulator" style={{ position: "relative", aspectRatio: "390 / 800", overflow: "hidden" }}>
      <iframe src={"https://app.revyl.ai/embed/4a22f507-3667-4162-b83f-1d7a7d4fa900?controls=0"} title="CardFields running on Android, in the React Native example app" loading="lazy" allow="fullscreen; clipboard-read; clipboard-write" style={{ position: "absolute", inset: 0, width: "100%", height: "100%", border: 0, background: "transparent", display: "block" }} />
    </div>
  </div>
</div>

<div data-whop-usage="payments/cardFields">
  <CodeGroup>
    ```tsx React theme={null}
    import { WhopElements, Payments, CardFields, CardNumberElement, CardExpiryElement, CardCvcElement } from "@whop/elements-react";
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
    <script src="https://js.whop.cloud/elements/amber/elements.js" data-whop-elements></script>
    <script type="module">
      const payments = window.WhopElements().payments.create({ /* options */ });
      const cardFields = payments.create('cardFields', { /* options */ });
      cardFields.create('cardNumber').mount('#payments-cardFields-cardNumber');
      cardFields.create('cardExpiry').mount('#payments-cardFields-cardExpiry');
      cardFields.create('cardCvc').mount('#payments-cardFields-cardCvc');
    </script>
    ```
  </CodeGroup>
</div>

<div data-whop-platform="web">
  ## Options

  Pass these to `payments.create('cardFields', { … })`, or as props on `<CardFields>` in React. Parent-injected props never appear here.

  <ResponseField name="layout" type="&#x22;compact&#x22; | &#x22;stacked&#x22;">
    `stacked` (default) shapes composed fields as a number row above expiration and security code. `compact` shapes one row. Separately mounted fields ignore this layout and render with full borders and rounded corners. Defaults to `"stacked"`.
  </ResponseField>

  <ResponseField name="publicKey" type="string">
    Advanced Basis Theory publishable key. Omit it to fetch the key automatically.
  </ResponseField>

  <ResponseField name="networks" type="CardNetworkArt[]">
    The seller's accepted card networks in display order — the matrix card entry's `networks` objects, whose API-served icons drive the number field's brand art. Omit it to fetch alongside the key.
  </ResponseField>

  ## Events

  Pass callbacks in the create options or React props.

  ### `onChange`

  Fires when completeness changes. `complete` becomes true after the buyer fills all three fields. Use it to enable confirmation. `brand` is the detected card network. `funding` is the detected funding type (`credit`, `debit`, or `prepaid`), `null` until the number identifies one. `issuingCountry` is the lowercase two-letter code of the country the card was issued in, `null` until the number identifies one.

  **Signature:** `((payload: { complete: boolean; brand: string; funding: string | null; issuingCountry: string | null; }) => void)`

  ## Methods

  Call these on the sub handle from `payments.create('cardFields', { … })`.

  ### `tokenize`

  Advanced method that tokenizes the three fields without creating a confirmation token. It emits `tokenized` to the payments controller. Validation errors appear on the failing field, and the method throws. Use `payments.createConfirmationToken()` to confirm a payment.

  **Signature:** `(input: { accountId?: string | undefined; }) => Promise<{ token: string; }>`

  ### `collect`

  `payments.createConfirmationToken()` calls this action for `cardFields`. Don't call it directly. It tokenizes the fields and returns the data used to create the confirmation token.

  **Signature:** `(input: { billingDetails?: { email?: string | undefined; name?: string | undefined; address?: { country?: string | undefined; line1?: string | undefined; city?: string | undefined; postal_code?: string | undefined; } | undefined; } | undefined; }) => Promise<{ paymentMethod: { type: string; category: string; card: { token: string; }; }; billingDetails: { email: string; name?: string | undefined; address?: { country?: string | undefined; line1?: string | undefined; city?: string | undefined; postal_code?: string | undefined; } | undefined; }; }>`

  ### `update`

  Merges new props and callbacks into the sub-controller.

  **Signature:** `(options: Partial<CardFieldsSubOptions>) => void`

  ### `destroy`

  Destroys the sub-controller and its elements, then frees its exclusive slot. A later `create("cardFields")` starts fresh.

  **Signature:** `() => void`
</div>

<div data-whop-platform="react-native" style={{ display: "none" }}>
  ## Props

  <ResponseField name="children" type="ReactNode" required>
    Your layout, with the three field elements somewhere inside it.
  </ResponseField>

  <ResponseField name="publicKey" type="string">
    An Advanced tokenizer publishable key. Omit it and the SDK fetches the account's key itself.
  </ResponseField>

  <ResponseField name="layout" type="'stacked' | 'compact'">
    Shapes fields that are composed together. Separately mounted fields ignore it and render with full borders. Defaults to `stacked`.
  </ResponseField>

  <ResponseField name="onChange" type="(payload: CardFieldsChangePayload) => void">
    Fires on every edit with the combined state of the mounted fields.
  </ResponseField>

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

  <Note>
    Wrap your app in `<WhopElements getToken={…}>` once, then mount `<Payments>` around the elements. See [Getting started](/elements/beta/getting-started) and [Appearance](/elements/beta/appearance).
  </Note>
</div>

## Elements

The elements this sub-controller mounts. Each has its own page:

<CardGroup cols={2}>
  <Card title="CardNumberElement" href="/elements/beta/payments/cardFields-cardNumber">
    PCI-isolated hosted card number field. Card numbers never reach the host page.
  </Card>

  <Card title="CardExpiryElement" href="/elements/beta/payments/cardFields-cardExpiry">
    PCI-isolated hosted card expiration field.
  </Card>

  <Card title="CardCvcElement" href="/elements/beta/payments/cardFields-cardCvc">
    PCI-isolated hosted card security code field.
  </Card>
</CardGroup>
