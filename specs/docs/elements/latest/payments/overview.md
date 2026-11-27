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

# Payments

> Collect a payment from a `plan_` ID or inline currency and amount, or save a payment method without charging with `mode: "setup"`. Mount PaymentElement, CardElement, or CardFields, then call `payments.createConfirmationToken` with billing details. Wallet selections open their sheet automatically. When the account offers in-page bank linking (`bank_debit.bank_connection` on its bank account payment method type), a bank account selection opens Plaid Link on the page too. A buyer who closes either one rejects the call with `payment_request_cancelled`, which your pay handler should treat as a cancel. Confirm the token server-side, then pass the payment's or setup intent's `client_secret` to `handleNextAction` for any pending step.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

```tsx React theme={null}
import { WhopElements, Payments } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Payments /* options */>
        {/* mount elements here */}
      </Payments>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const payments = window.WhopElements().payments.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.payments.create({ … })`, or as props on `<Payments>` in React.

> [!NOTE]
>
> Pass exactly one of these combinations:
>
> * `accountId` and `plan`
> * `accountId`, `currency`, and `amount`
> * `accountId`, `mode: "setup"`, and `currency`
> * none of `plan`, `currency`, `amount`, or `mode`

**accountId** `string`

Account ID, prefixed `biz_`. Omit only when calling static methods without an account surface.

**mode** `"payment" | "setup"`

Mount mode, `payment` by default. `setup` saves a payment method and charges nothing, now or later: only methods that can be saved are offered and amount bounds do not apply. Confirm with `createConfirmationToken()`, then create the setup intent server-side.

**plan** `string`

Plan ID, prefixed `plan_`. It resolves client-side to currency, amount, and method configuration. Pass a plan or inline currency and amount, never both.

**offerAmounts** `object`

Minor-unit `feeInclusive` and `feeFree` totals used to gate methods when checkout re-prices `amount` for a fee-free method such as BNPL. Each method is checked against its matching total, preventing the selected fee-free method from disappearing. When omitted, `amount` gates every method.

**properties**

**feeInclusive** `number` *required*

**feeFree** `number` *required*

**returnUrl** `string`

Return URL for off-site bank or 3DS steps. Defaults to the mounting page. The buyer lands there whatever the outcome, with `payment` (or `setup`), `status` (`succeeded`, `failed`, `canceled`, or a still-pending status such as `processing`), and `client_secret` appended, so branch on `status` before showing a success page.

**setupFutureUsage** `"off_session" | "on_session"`

A setup saves the payment method for `off_session` use, the only usage a setup accepts.

**previewWalletAvailability** `WalletAvailability`

Wallet availability override for deterministic previews. Live checkouts probe the host.

**properties**

**applePay** `boolean` *required*

**googlePay** `boolean` *required*

**payPal** `boolean`

**order** `("apple_pay" | "google_pay" | "paypal_express")[]`

The wallets ranked by which sheet is native to this browser — Apple Pay first on WebKit, Google Pay first everywhere else, PayPal last — so express buttons can stack best-first. Absent on consumer-stated availability (previews) and on wires that predate it. An inline union, not payment-request's WalletSheetType: that module already imports this one.

**wallets** `object`

Each available wallet's details, keyed by wallet type. Absent on consumer-stated availability (previews).

**properties**

**apple\_pay** `WalletDetails`

**properties**

**button** `WalletButtonArt`

The wallet's official button art. Absent when none is published.

**properties**

**light** `object` *required*

For light pages.

**properties**

**color** `string` *required*

**image** `string` *required*

**dark** `object` *required*

For dark pages.

**properties**

**color** `string` *required*

**image** `string` *required*

**google\_pay** `WalletDetails`

**properties**

**button** `WalletButtonArt`

The wallet's official button art. Absent when none is published.

**properties**

**light** `object` *required*

For light pages.

**properties**

**color** `string` *required*

**image** `string` *required*

**dark** `object` *required*

For dark pages.

**properties**

**color** `string` *required*

**image** `string` *required*

**paypal\_express** `WalletDetails`

**properties**

**button** `WalletButtonArt`

The wallet's official button art. Absent when none is published.

**properties**

**light** `object` *required*

For light pages.

**properties**

**color** `string` *required*

**image** `string` *required*

**dark** `object` *required*

For dark pages.

**properties**

**color** `string` *required*

**image** `string` *required*

**currency** `string`

Lowercase three-letter ISO 4217 currency code. It filters the payment-method matrix.

**amount** `number`

Payment amount in minor units. Methods whose bounds exclude it aren't offered.

**paymentMethodConfiguration** `object`

Account payment-method filters in the API's `enabled`, `disabled`, and `include_platform_defaults` format. `disabled` removes types, `enabled` restores offered types, and `include_platform_defaults: false` leaves only enabled types. Plans provide their own configuration.

**properties**

**enabled** `string[]`

**disabled** `string[]`

**include\_platform\_defaults** `boolean`

**appearance** `Appearance`

Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.

**locale** `WhopElementsLocale`

Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.

## Events

Pass callbacks in the create options or React props.

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**loading** `boolean`

## Methods

Call these on the Payments handle from `whop.payments.create({ … })` or `usePayments()`.

### `createConfirmationToken`

Creates a confirmation token for the selected method from your pay button. Pass the checkout's billing details. They must include `email`. Supply `phone` for methods that require it when billing collection is disabled. Cards also need `name` and an address containing `line1` and `country`. Wallet selections open their sheet during the pay-button interaction. When the account offers in-page bank linking (`bank_debit.bank_connection` on its bank account payment method type), a bank account selection opens Plaid Link during it too, and the call resolves once the buyer has linked an account. That needs a whop.com page or a domain with Apple Pay ready through the [Domains API](https://docs.whop.com/api-reference/beta/domains/domain), and a Content Security Policy that allows the Plaid hosts `contentSecurityPolicy()` returns. Otherwise the buyer links the account after submitting instead. If the buyer closes the sheet or Link, the call rejects with `payment_request_cancelled`: treat it as a cancel, not a payment error, and let the buyer pay again. Confirm the returned `confirmationToken` server-side, then call `WhopElements.payments.handleNextAction(…)`. Runs on your page (host-side), not in the controller iframe.

**Parameters**

**input** `object` *required*

**properties**

**billingDetails** `object`

**properties**

**email** `string`

**name** `string`

**phone** `string`

**address** `object`

**properties**

**country** `string`

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**Returns** `Promise<object>`

**confirmationToken** `string` *required*

**type** `string` *required*

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<PaymentsOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Static methods

Call these without mounting: `whop.payments.<method>(…)` in vanilla or `useWhop().payments.<method>(…)` in React.

### `handleNextAction`

Completes a confirmed payment's pending step without mounting an element. `clientSecret` identifies the payment. You don't need a payment ID. Inline steps open in a dialog and resolve with `redirected: false`. Dismissing the dialog returns the payment's current status, which can remain pending. Full-page steps redirect with `redirected: true` to `return_url`, or a hosted receipt when the payment has no return URL. Inside an iframe they navigate the top-level tab. If the embedding page refuses that navigation, they reject with code `TAB_NAVIGATION_BLOCKED`, and the payment stays pending. Set `returnUrl` to update the destination before presentation. An update failure rejects before any step runs. Inline steps never navigate to that URL. A payment whose charge is still being decided is polled until a step appears or it settles; a decided payment with no step resolves immediately.

**Parameters**

**input** `object` *required*

**properties**

**clientSecret** `string` *required*

**returnUrl** `string`

**pollIntervalMs** `number`

**Returns** `Promise<object>`

**status** `string` *required*

**redirected** `boolean` *required*

**lastPaymentError** `object | null` *required*

**properties**

**code** `string | null`

**decline\_code** `string | null`

**message** `string | null`

## Resources

Create these SDK objects without mounting an element. Each has its own page:

> [card] [PaymentRequest](https://docs.whop.com/elements/latest/payments/paymentRequest.md)
>
> Low-level Apple Pay, Google Pay, or PayPal sheet for custom buttons, express checkout, and shipping callbacks. Wallet tiles automate this flow through `payments.createConfirmationToken`. Await `canMakePayment()` to check availability and prime the wallet, then call `show(type)` synchronously in the user-interaction handler. The resolved `ctok` is a `ctok_`-prefixed confirmation token. Confirm it server-side like an element-minted token. A PayPal token charges at most the total PayPal showed the buyer.

## Content security policy

Some of this group's work runs on your page, not in a frame. A page with a Content Security Policy needs these sources. `contentSecurityPolicy()` from `@whop/elements` returns every source in this table. The sandbox source applies with `environment: "sandbox"`.

| Requirement | Directive | Production source | Sandbox source |
| - | - | - | - |
| Plaid Link's script | `script-src` | `https://cdn.plaid.com` | Same |
| ↳ Plaid Link's frame, which Plaid's script opens | `frame-src` | `https://cdn.plaid.com` | Same |
| ↳ Plaid's API, which Plaid's script calls | `connect-src` | `https://production.plaid.com` | `https://sandbox.plaid.com` |

## Elements

The elements this group mounts. Each has its own page:

> [card] [AddressElement](https://docs.whop.com/elements/latest/payments/address.md)
>
> Collects a billing or shipping address. Fields, order, and validation follow the selected country. Includes street autocomplete (full screen on phones) and methods to read or validate the address.

> [card] [EmailElement](https://docs.whop.com/elements/latest/payments/email.md)
>
> Collects the buyer's email and passes it to `payments.createConfirmationToken()` while mounted. Explicit `billingDetails.email` wins. A matching Whop account shows optional sign-in with code verification, or with the account's passkey where it has one. Successful sign-in unlocks saved payment methods in the payment element. Buyers can continue as guests.

> [card] [TaxIdElement](https://docs.whop.com/elements/latest/payments/taxId.md)
>
> Collects a business tax registration accepted by the API. Labels use buyer-facing names. The placeholder matches the selected format. `country` preselects a type. `onChange` emits committed pairs. The host supplies API validation errors.

> [card] [BrandingElement](https://docs.whop.com/elements/latest/payments/branding.md)
>
> **Required — the other elements except `AddressElement`, `EmailElement`, and `TaxIdElement` wait for it.** Whop's merchant-of-record notice: the buyer's agreement to Whop's Buyer Terms of Service and Privacy Policy, with the Whop wordmark. Required — the payment, card and card fields elements stay in their loading state until it is mounted, and payments cannot be confirmed without it.

**Alternatives.** Mount one at a time. Destroy it before mounting another:

> [card] [PaymentElement](https://docs.whop.com/elements/latest/payments/payment.md)
>
> Shows available payment methods and collects the selected method's required fields and disclosures. Use `change` to enable your pay button. In its activation handler, call `payments.createConfirmationToken()`. Confirm the token server-side, then pass any pending step to `payments.handleNextAction(…)`. Use `addressChange` for address-dependent updates.

> [card] [CardElement](https://docs.whop.com/elements/latest/payments/card.md)
>
> Prearranged fields for card number, expiration, and security code. Create with `payments.create("card")`, enable your payment button from `onChange`, and confirm with `payments.createConfirmationToken()`. Card numbers remain in PCI-isolated hosted fields. `layout` supports `stacked` (default) and `compact`.

> [card] [CardFields](https://docs.whop.com/elements/latest/payments/cardFields)
>
> Three separately mountable, PCI-isolated card fields for custom layouts: number, expiration, and security code. Create with `payments.create("cardFields")`, place each field, enable your payment button from `onChange`, and confirm with `payments.createConfirmationToken()`. Card numbers remain in hosted fields. *(sub-controller, 3 elements)*


## Related topics

- [Payment](https://docs.whop.com/api-reference/legacy/payments/payment.md)
- [Payments API](https://docs.whop.com/api-reference/payments/payment.md)
- [Accept Payments](https://docs.whop.com/developer/guides/accept-payments.md)
