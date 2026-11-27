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

# PaymentRequest

> Low-level Apple Pay, Google Pay, or PayPal sheet for custom buttons, express checkout, and shipping callbacks. Wallet tiles automate this flow through `payments.createConfirmationToken`. Await `canMakePayment()` to check availability and prime the wallet, then call `show(type)` synchronously in the user-interaction handler. The resolved `ctok` is a `ctok_`-prefixed confirmation token. Confirm it server-side like an element-minted token. A PayPal token charges at most the total PayPal showed the buyer.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

Create this [`Payments`](https://docs.whop.com/elements/latest/payments/overview) resource without mounting an element: `whop.payments.paymentRequest.create({ … })` in vanilla or `useWhop().payments.paymentRequest.create({ … })` in React.

## Options

Pass these to `whop.payments.paymentRequest.create({ … })`.

**accountId** `string` *required*

Account ID, prefixed `biz_`, that scopes every client-side call.

**currency** `string` *required*

Three-letter ISO 4217 payment currency code.

**amount** `number` *required*

Payment amount in minor units. Change it later with `update()`.

**setupFutureUsage** `"off_session" | "on_session"`

Set only after displaying save consent. Marks the token for `off_session` or `on_session` use.

**paymentMethodConfiguration** `PaymentMethodConfigurationShape`

Payment method filters in the API's `enabled`, `disabled`, and `include_platform_defaults` format. They narrow the wallets `canMakePayment()` reports: `disabled` removes types, `enabled` restores offered types, and `include_platform_defaults: false` leaves only enabled types. They never add a method the account doesn't offer. PayPal follows the `paypal` type. Since `v1.9.0`.

**properties**

**enabled** `string[]`

**disabled** `string[]`

**include\_platform\_defaults** `boolean`

**requestPayerEmail** `boolean`

Ask the sheet for the payer's email — the confirmation token requires one. Defaults to `true`.

**requestPayerPhone** `boolean`

Ask the sheet for the payer's phone number, delivered as `payer.phone` on the result — in E.164 where it parses against the country of the contact that supplied it, as the wallet gave it otherwise; `payer.country` is the billing country. Google Pay collects it through the billing address the sheet already asks for. PayPal collects it through its contact section and refuses an account without one. Defaults to `false`.

**requestShipping** `boolean`

Ask the sheet for a shipping address (and offer `shippingOptions` when given). PayPal always asks for an address, since it is the address tax is computed on, and returns it as `shipping` only when this is set. Defaults to `false`.

**requestPromoCode** `boolean`

Offer the sheet's own promo code field and answer `onPromoCodeChange`. Apple Pay shows the field on devices that support Apple Pay version 12 (iOS 15 and macOS 12); Google Pay shows it where the account's Google Pay setup allows offers. Other devices open the sheet without it. Defaults to `false`. Since `v1.5.0`.

**appliedPromoCode** `string`

A promo code already applied to the charge, prefilled into the sheet so the buyer can see, replace or remove it. Since `v1.5.0`.

**appliedPromoDescription** `string`

What `appliedPromoCode` does, shown next to the code in Google Pay. Apple Pay's coupon field has no caption. Since `v1.5.0`.

**countryCode** `string`

Account ISO 3166-1 alpha-2 country code for the Apple Pay sheet. Omit it to use the account's published registration country. The sheet defaults to `US` when neither is available.

**lineItems** `PaymentRequestLineItem[]`

Line items the sheet lists under the total. Change it later with `update()`.

**properties**

**label** `string` *required*

**amount** `number` *required*

Minor units, same as `amount`.

**shippingOptions** `PaymentRequestShippingOption[]`

Shipping options the sheet offers when `requestShipping` is set. Display-only — update `amount` from your change handler. The sheet opens with the first option selected and fires no `onShippingOptionChange` for it, so `amount` must already include its price. Change it later with `update()`.

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

## Methods

Call these on the resource.

> [!WARNING]
>
> Call `show` synchronously during user activation. Await prerequisites first.

### `canMakePayment`

Returns availability for each wallet when the account offers it, the device supports it, and this page's origin is approved for the account (first-party whop.com pages are pre-approved; a merchant page needs Apple Pay ready on its domain through the [Domains API](https://docs.whop.com/api-reference/beta/domains/domain), and offering Google Pay there is subject to the [Google Pay API Terms of Service](https://payments.developers.google.com/terms/sellertos)). On a page that is itself inside an iframe, Apple Pay checks the top-level page's domain instead when the browser exposes it. Apple Pay on non-WebKit browsers is the desktop iPhone-handoff flow, so mobile devices there report it unavailable — native Apple Pay on iOS browsers is unaffected. `order` ranks the wallets by which sheet is native to the browser — Apple Pay first on Safari, Google Pay first elsewhere, PayPal last — so express buttons can stack best-first. `payPal` reports PayPal where the account offers it in this currency, the amount is within PayPal's limits, the request does not ask for the payer's phone (an `off_session` request may be \$0, which saves PayPal without a charge), PayPal's scripts load on this page, and this page's origin is approved for the account like the other wallets'. A page with a Content Security Policy must allow PayPal's hosts, which `contentSecurityPolicy()` includes. A page that already loads PayPal's own v5 JavaScript SDK reports `payPal: false`. Pass `types` to check only the wallets you list; `show(type)` needs that wallet checked. `wallets` carries each available wallet's official button art (`light` and `dark` color and image). This primes `show()`. Always await it first.

**Parameters**

**filter** `object`

**properties**

**types** `("apple_pay" | "google_pay" | "paypal_express")[]`

**Returns** `Promise<WalletAvailability>`

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

### `show`

Open the selected wallet after awaiting `canMakePayment()`, synchronously in the press: Apple requires session creation within the gesture stack, and PayPal opens its window there — a pop-up on desktop, PayPal's in-page window when the pop-up is blocked and on mobile. Pass `email` when your page already collected one — the wallet takes it and never asks for a second. With `requestPayerEmail: false`, passing one is required, and the wallet refuses to open without it because confirmation tokens require an email. While the sheet is open, `update()` throws: answer the change events to change it. Call synchronously during user activation. Await prerequisites first because browsers revoke activation across asynchronous steps.

**Parameters**

**type** `"apple_pay" | "google_pay" | "paypal_express"` *required*

**provided** `object`

**properties**

**email** `string`

**Returns** `Promise<PaymentRequestResult>`

**ctok** `string` *required*

Confirmation token ID, prefixed `ctok_`. Confirm it server-side with your secret key.

**type** `"apple_pay" | "google_pay" | "paypal_express"` *required*

**payer** `object` *required*

`phone` is E.164 where it parsed against the contact's country, as given otherwise; PayPal gives one only for `requestPayerPhone`, and refuses an account without one. `country` is the billing country every sheet collects.

**properties**

**email** `string`

**name** `string`

**phone** `string`

**country** `string`

**billing** `object | null` *required*

The full billing address the confirmation token carries, unlike the redacted one the change events carry; `null` when the wallet gave none. Apple Pay and Google Pay give the card's billing address. On PayPal it is the address the buyer picked in PayPal, the one its total was taxed on.

**properties**

**address** `PaymentRequestAddress` *required*

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**shipping** `object | null` *required*

`null` unless `requestShipping` was set. `option` is the selected shipping option: the first offered one until the buyer picks another, and the first again when an answer replaced the options without the picked `id`. Apple Pay and Google Pay set it whenever options were offered, and PayPal never does.

**properties**

**address** `PaymentRequestAddress`

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**option** `PaymentRequestShippingOption`

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

### `update`

Change `amount`, `lineItems` and `shippingOptions` on this instance. Every other option is fixed at `create()`: create a new instance to change it.

**Parameters**

**patch** `object` *required*

**properties**

**amount** `number`

**lineItems** `PaymentRequestLineItem[]`

**properties**

**label** `string` *required*

**amount** `number` *required*

Minor units, same as `amount`.

**shippingOptions** `PaymentRequestShippingOption[]`

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

## Events

Subscribe to events below. Each method returns an unsubscribe function.

> [!WARNING]
>
> Each event handler must call its documented reply method exactly once. A handler that throws or rejects before replying fails that update right away.

### `onShippingAddressChange`

The buyer chose a shipping address (redacted pre-authorization: city, state, postal code and country only; Apple Pay cuts a US ZIP code to five digits and drops the last three characters of a Canadian or UK postal code). Answer with the totals for the address and the selected shipping option: the first one until the buyer picks another. `shippingOptions` replaces the options (an empty list changes nothing). A picked option stays selected while the answer keeps its `id`; when the answer drops it, the first option becomes the selection without an `onShippingOptionChange`, so price that one. Every error in the answer stays on the address until a later answer for an address clears it: one that names a `field` (`city`, `state`, `postal_code`, `country`) marks that field in Apple Pay, and Google Pay shows the message alone. Pay is refused while an error stands on the address. Refuse an address you can't ship to with a `shipping_address_unserviceable` error and no `field`. On Apple Pay, Google Pay and PayPal, a handler that hasn't answered within 25 seconds fails the address, the way a throwing one does, and its later answer is ignored. PayPal fires it with the address the buyer picks, and an error naming a `field` refuses that part of the address inside PayPal. Call `updateWith(…)` exactly once per event. A handler that throws or rejects before calling it fails that update right away. Returns the unsubscribe function.

**ev** `ShippingAddressChangeEvent`

**properties**

**address** `PaymentRequestAddress` *required*

Vendor-normalized pre-authorization address. It includes only city, state, postal code, and country. Apple Pay cuts the postal code down: a US ZIP code to its first five digits, and a Canadian or UK postal code without its last three characters.

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**updateWith** `(update: PaymentRequestUpdate) =\> void` *required*

Call this exactly once, within 25 seconds. On Apple Pay, Google Pay and PayPal a handler that hasn't answered by then fails the address the way a throwing one does, and its later answer is ignored.

**parameters**

**update** `PaymentRequestUpdate` *required*

**properties**

**amount** `number`

**lineItems** `PaymentRequestLineItem[]`

**properties**

**label** `string` *required*

**amount** `number` *required*

Minor units, same as `amount`.

**shippingOptions** `PaymentRequestShippingOption[]`

Replaces the sheet's shipping options. The selected option stays selected while its `id` is still in the list; otherwise the first option becomes the selection, with no `shippingOptionChange`, so price it in the same answer. An empty list changes nothing: refuse an address you can't ship to with a `shipping_address_unserviceable` error instead.

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

**errors** `PaymentRequestError[]`

**properties**

**code** `string`

What a shipping answer refuses. An error in a `billingAddressChange` or `paymentMethodChange` answer refuses the card or method that event asked about, whatever its code.

One of `"other"`, `"shipping_address_invalid"`, `"shipping_address_unserviceable"`, or `"shipping_option_invalid"`.

**message** `string` *required*

**field** `"state" | "country" | "city" | "postal_code"`

The address field the error is about: one of the fields the redacted address carries. Apple Pay marks that field with the message, whatever the `code`; Google Pay shows the message alone. Omit it for an error about the whole address.

### `onShippingOptionChange`

The buyer picked a shipping option, or Apple Pay re-announced the selected one: once the address changes are all answered and none is waiting, Apple Pay fires it for the option still selected. It never fires after an answer that drops the selected option, which makes the first option the selection without an event, so every total you answer prices the selected option: the first one until the buyer picks another, the pick while your answers keep its `id`, and the first again once an answer drops it. Amount-only contract: `updateWith` carries amount/lineItems/shippingOptions/errors. An answer with `errors` refuses the option until the buyer picks one your handler accepts. Apple Pay has no error slot for a shipping option, so it keeps the last accepted total whatever else the answer carries, and the first error's message shows when the buyer taps Pay. Google Pay shows the message when the option is picked. On Apple Pay, Google Pay and PayPal, a handler that hasn't answered within 25 seconds refuses the option, the way a throwing one does, and its later answer is ignored. Call `updateWith(…)` exactly once per event. A handler that throws or rejects before calling it fails that update right away. Returns the unsubscribe function.

**ev** `ShippingOptionChangeEvent`

**properties**

**option** `object & Partial\<PaymentRequestShippingOption\>` *required*

Always includes the selected `id`. It includes the full option when it matches one of yours.

**properties**

**id** `string` *required*

**label** `string`

**detail** `string`

**amount** `number`

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

**updateWith** `(update: PaymentRequestUpdate) =\> void` *required*

Call this exactly once, within 25 seconds. On Apple Pay, Google Pay and PayPal a handler that hasn't answered by then refuses the option the way a merchant refusal does, and its later answer is ignored.

**parameters**

**update** `PaymentRequestUpdate` *required*

**properties**

**amount** `number`

**lineItems** `PaymentRequestLineItem[]`

**properties**

**label** `string` *required*

**amount** `number` *required*

Minor units, same as `amount`.

**shippingOptions** `PaymentRequestShippingOption[]`

Replaces the sheet's shipping options. The selected option stays selected while its `id` is still in the list; otherwise the first option becomes the selection, with no `shippingOptionChange`, so price it in the same answer. An empty list changes nothing: refuse an address you can't ship to with a `shipping_address_unserviceable` error instead.

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

**errors** `PaymentRequestError[]`

**properties**

**code** `string`

What a shipping answer refuses. An error in a `billingAddressChange` or `paymentMethodChange` answer refuses the card or method that event asked about, whatever its code.

One of `"other"`, `"shipping_address_invalid"`, `"shipping_address_unserviceable"`, or `"shipping_option_invalid"`.

**message** `string` *required*

**field** `"state" | "country" | "city" | "postal_code"`

The address field the error is about: one of the fields the redacted address carries. Apple Pay marks that field with the message, whatever the `code`; Google Pay shows the message alone. Omit it for an error about the whole address.

### `onBillingAddressChange`

The buyer selected a card, with its redacted billing address (city, state, postal code and country only): reprice the total for the address the charge taxes off. Apple Pay fires it when the buyer selects a card, and Google Pay when the sheet opens and each time the buyer switches cards. Answer `updateWith({ amount })` in minor units, `updateWith({})` to keep the current total, or `updateWith({ errors })` to refuse the card. Every event is answered exactly once: a handler that throws, rejects, or doesn't answer within 10 seconds refuses the card, so the buyer picks another one. On Apple Pay and Google Pay the same selection then fires `paymentMethodChange`, and the sheet updates once both are answered. PayPal fires it with each shipping address the buyer picks in PayPal, which is the address its token bills and taxes on, and a refusal there refuses that address inside PayPal. Call `updateWith(…)` exactly once per event. A handler that throws or rejects before calling it fails that update right away. Returns the unsubscribe function.

**ev** `BillingAddressChangeEvent`

**properties**

**address** `PaymentRequestAddress` *required*

Vendor-normalized pre-authorization billing address. It includes only city, state, postal code, and country.

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**updateWith** `(update: PaymentRequestUpdate) =\> void` *required*

Call this exactly once. An event left unanswered for 10 seconds refuses the card, and the buyer picks another one.

**parameters**

**update** `PaymentRequestUpdate` *required*

**properties**

**amount** `number`

**lineItems** `PaymentRequestLineItem[]`

**properties**

**label** `string` *required*

**amount** `number` *required*

Minor units, same as `amount`.

**shippingOptions** `PaymentRequestShippingOption[]`

Replaces the sheet's shipping options. The selected option stays selected while its `id` is still in the list; otherwise the first option becomes the selection, with no `shippingOptionChange`, so price it in the same answer. An empty list changes nothing: refuse an address you can't ship to with a `shipping_address_unserviceable` error instead.

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

**errors** `PaymentRequestError[]`

**properties**

**code** `string`

What a shipping answer refuses. An error in a `billingAddressChange` or `paymentMethodChange` answer refuses the card or method that event asked about, whatever its code.

One of `"other"`, `"shipping_address_invalid"`, `"shipping_address_unserviceable"`, or `"shipping_option_invalid"`.

**message** `string` *required*

**field** `"state" | "country" | "city" | "postal_code"`

The address field the error is about: one of the fields the redacted address carries. Apple Pay marks that field with the message, whatever the `code`; Google Pay shows the message alone. Omit it for an error about the whole address.

### `onPaymentMethodChange`

The buyer selected a payment method: reprice the total for it, such as a surcharge on credit cards. Apple Pay fires it when the buyer selects a card, and Google Pay when the sheet opens and each time the buyer switches cards, each right after that card's `billingAddressChange`. `paymentMethod.type` is `card`. `paymentMethod.card` carries what the wallet tells about a card before the buyer authorizes: Apple Pay reports its `funding` (Apple usually withholds the network until then), and Google Pay its `network`. `paymentMethod.billingAddress` is the card's redacted billing address. Answer `updateWith({ amount, lineItems })` to reprice, `updateWith({})` to keep the current total, or `updateWith({ errors })` to refuse the method; `shippingOptions` is ignored. Apple Pay marks a refusal on the card's billing contact (on the named `field` when one is given) and holds its Pay button until the buyer picks another method. Google Pay shows the message on the card and refuses a Pay tap on it until the buyer picks a card you accept. A refused card leaves the totals as they were, whatever this answer or its `billingAddressChange` answer priced. Every event is answered exactly once: a handler that throws, rejects, or doesn't answer within 10 seconds refuses the method. Each sheet waits for your answer before the buyer can pay. PayPal never fires it. Call `updateWith(…)` exactly once per event. A handler that throws or rejects before calling it fails that update right away. Returns the unsubscribe function.

**ev** `PaymentMethodChangeEvent`

**properties**

**paymentMethod** `PaymentRequestPaymentMethod` *required*

The method the buyer selected, as the wallet describes it before authorization.

**properties**

**type** `string` *required*

`card` on Apple Pay and Google Pay, which report only cards; `paypal` on PayPal.

**card** `object`

What the wallet tells about the card. Before the buyer authorizes, on `paymentMethodChange`, Apple Pay reports its funding, usually without the network, and Google Pay its network. At authorization Apple Pay and Google Pay report both.

**properties**

**network** `string`

**funding** `"credit" | "debit" | "prepaid" | "store"`

**billingAddress** `PaymentRequestAddress`

The card's redacted billing address (city, state, postal code and country only), which Apple Pay and Google Pay hand over with the card.

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**displayName** `string`

The wallet's own label for the method, such as `Visa 1234`. Apple Pay and Google Pay set it only once the buyer authorizes, so only `paymentAuthorization` carries it. Show it, and don't parse it: the wallets promise no format.

**updateWith** `(update: PaymentRequestUpdate) =\> void` *required*

Call this exactly once: `amount` and `lineItems` reprice the sheet for the method, `updateWith({})` keeps the total, and `errors` refuses the method. An event left unanswered for 10 seconds refuses the method.

**parameters**

**update** `PaymentRequestUpdate` *required*

**properties**

**amount** `number`

**lineItems** `PaymentRequestLineItem[]`

**properties**

**label** `string` *required*

**amount** `number` *required*

Minor units, same as `amount`.

**shippingOptions** `PaymentRequestShippingOption[]`

Replaces the sheet's shipping options. The selected option stays selected while its `id` is still in the list; otherwise the first option becomes the selection, with no `shippingOptionChange`, so price it in the same answer. An empty list changes nothing: refuse an address you can't ship to with a `shipping_address_unserviceable` error instead.

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

**errors** `PaymentRequestError[]`

**properties**

**code** `string`

What a shipping answer refuses. An error in a `billingAddressChange` or `paymentMethodChange` answer refuses the card or method that event asked about, whatever its code.

One of `"other"`, `"shipping_address_invalid"`, `"shipping_address_unserviceable"`, or `"shipping_option_invalid"`.

**message** `string` *required*

**field** `"state" | "country" | "city" | "postal_code"`

The address field the error is about: one of the fields the redacted address carries. Apple Pay marks that field with the message, whatever the `code`; Google Pay shows the message alone. Omit it for an error about the whole address.

### `onPromoCodeChange`

The buyer typed, changed, or removed a code in the sheet's promo field (`requestPromoCode`). Answer `updateWith({ amount })` to apply the code at a new total, `updateWith({})` to apply it at the current total, or `updateWith({ error })` to refuse it in place. A removal arrives with `code` null and is answered the same way. Replacing an applied code arrives as one event with the new code, never a removal first, and a refused new code leaves the applied one in place. When `shippingOptions` drops the selected option, the first one becomes the selection without an `onShippingOptionChange`, so price that one. `appliedDescription` captions the code in Google Pay; Apple Pay's coupon field has no caption, so put anything the buyer must see in a line item label. A handler that throws, rejects, or doesn't answer within 10 seconds refuses the code. Call `updateWith(…)` exactly once per event. A handler that throws or rejects before calling it fails that update right away. Returns the unsubscribe function.

**ev** `PromoCodeChangeEvent`

**properties**

**code** `string | null` *required*

The code now in the sheet's promo field, or null when the buyer removed it. Replacing an applied code is one event with the new code, never a removal first.

**updateWith** `(update: PromoCodeUpdate) =\> void` *required*

Answer exactly once: `error` refuses the code and keeps the totals, and any other answer applies it. `amount` and `lineItems` reprice the sheet under the code, `shippingOptions` replaces its shipping options (a free-shipping code), and `appliedDescription` captions the applied code where the sheet shows one (Google Pay). `updateWith({})` applies the code at the current total.

**parameters**

**update** `PromoCodeUpdate` *required*

**properties**

**amount** `number`

The total under the code, in minor units.

**lineItems** `PaymentRequestLineItem[]`

The sheet's line items under the code, such as the discount and the tax it changes.

**properties**

**label** `string` *required*

**amount** `number` *required*

Minor units, same as `amount`.

**shippingOptions** `PaymentRequestShippingOption[]`

Replaces the sheet's shipping options under the code, such as a free-shipping code that drops a rate to zero. An option stays selected while its `id` is still in the list; otherwise the first option becomes the selection, with no `shippingOptionChange`. Send `amount` as well when the selected option's price changes, since the total doesn't follow on its own. An empty list changes nothing.

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

**appliedDescription** `string`

A caption shown beside the applied code in Google Pay. Apple Pay's coupon field has no caption, so put anything the buyer must see in a line item label.

**error** `object`

Refuses the code. The sheet shows `message` next to its promo field.

**properties**

**message** `string` *required*

### `onPaymentAuthorization`

The buyer authorized the payment, and no confirmation token exists yet. Subscribe to approve or refuse it first: the sheet waits for your answer, and without a handler the token is created at once. The event carries the payer, the payment method (with its redacted billing address, and in `displayName` the wallet's own label for it, such as `Visa 1234`), the shipping address and option, and the total and line items the buyer authorized. Answer `respondWith({ approved: true })` to create the token, which `show()` and `onConfirmationToken` then deliver as usual. Answer `respondWith({ approved: false, message })` to refuse: no token is created, Apple Pay and Google Pay show `message` in the sheet, and the buyer can change something and pay again, or close the sheet. PayPal can't show your message, so a refusal there ends the attempt: `show()` rejects with `payment_request_refused`, and the buyer can press PayPal again. Every event is answered exactly once, and only `approved: true` creates a token: a handler that throws, rejects, or doesn't answer within 25 seconds refuses the payment, early enough for Apple Pay and Google Pay to show the refusal before their own 30-second limit. Call `respondWith(…)` exactly once per event. A handler that throws or rejects before calling it fails that update right away. Returns the unsubscribe function.

**ev** `PaymentAuthorizationEvent`

**properties**

**type** `"apple_pay" | "google_pay" | "paypal_express"` *required*

The wallet the buyer authorized in.

**payer** `object` *required*

The payer, as the result will carry it.

**properties**

**email** `string`

**name** `string`

**phone** `string`

**country** `string`

**paymentMethod** `PaymentRequestPaymentMethod` *required*

The method the buyer authorized. Its billing address is redacted to city, state, postal code and country, as `paymentMethodChange` carries it.

**properties**

**type** `string` *required*

`card` on Apple Pay and Google Pay, which report only cards; `paypal` on PayPal.

**card** `object`

What the wallet tells about the card. Before the buyer authorizes, on `paymentMethodChange`, Apple Pay reports its funding, usually without the network, and Google Pay its network. At authorization Apple Pay and Google Pay report both.

**properties**

**network** `string`

**funding** `"credit" | "debit" | "prepaid" | "store"`

**billingAddress** `PaymentRequestAddress`

The card's redacted billing address (city, state, postal code and country only), which Apple Pay and Google Pay hand over with the card.

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**displayName** `string`

The wallet's own label for the method, such as `Visa 1234`. Apple Pay and Google Pay set it only once the buyer authorizes, so only `paymentAuthorization` carries it. Show it, and don't parse it: the wallets promise no format.

**shipping** `object | null` *required*

`null` unless `requestShipping` was set. Otherwise the address and option the result will carry.

**properties**

**address** `PaymentRequestAddress`

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**option** `PaymentRequestShippingOption`

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

**amount** `number` *required*

The total the buyer authorized, in minor units.

**lineItems** `PaymentRequestLineItem[]` *required*

The line items the sheet showed under that total.

**properties**

**label** `string` *required*

**amount** `number` *required*

Minor units, same as `amount`.

**respondWith** `(response: PaymentAuthorizationResponse) =\> void` *required*

Call this exactly once. `{ approved: true }` creates the confirmation token. Anything else refuses the payment, and no token is created.

**parameters**

**response** `PaymentAuthorizationResponse` *required*

**properties**

**approved** `boolean` *required*

`true` creates the confirmation token. `false` refuses the payment.

**message** `string`

Shown to the buyer when you refuse, in the sheet that took the authorization.

### `onConfirmationToken`

The sheet created a confirmation token, heard once the sheet has closed. This is the same result that `show()` returns. Returns the unsubscribe function.

**ev** `PaymentRequestResult`

**properties**

**ctok** `string` *required*

Confirmation token ID, prefixed `ctok_`. Confirm it server-side with your secret key.

**type** `"apple_pay" | "google_pay" | "paypal_express"` *required*

**payer** `object` *required*

`phone` is E.164 where it parsed against the contact's country, as given otherwise; PayPal gives one only for `requestPayerPhone`, and refuses an account without one. `country` is the billing country every sheet collects.

**properties**

**email** `string`

**name** `string`

**phone** `string`

**country** `string`

**billing** `object | null` *required*

The full billing address the confirmation token carries, unlike the redacted one the change events carry; `null` when the wallet gave none. Apple Pay and Google Pay give the card's billing address. On PayPal it is the address the buyer picked in PayPal, the one its total was taxed on.

**properties**

**address** `PaymentRequestAddress` *required*

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**shipping** `object | null` *required*

`null` unless `requestShipping` was set. `option` is the selected shipping option: the first offered one until the buyer picks another, and the first again when an answer replaced the options without the picked `id`. Apple Pay and Google Pay set it whenever options were offered, and PayPal never does.

**properties**

**address** `PaymentRequestAddress`

**properties**

**name** `string`

The name on the address.

**line1** `string`

**line2** `string`

**city** `string`

**state** `string`

**postal\_code** `string`

**country** `string`

**recipient** `string` *deprecated*

> [!WARNING]
>
> Deprecated. Use `name`.

The same value as `name`.

**phone** `string`

**option** `PaymentRequestShippingOption`

**properties**

**id** `string` *required*

**label** `string` *required*

**detail** `string`

**amount** `number` *required*

Minor-unit display value. The resource doesn't calculate shipping. Update `amount` from your handler.

### `onCancel`

The buyer dismissed the sheet or closed PayPal. Returns the unsubscribe function.

## Content security policy

This resource runs on your page, not in a frame. A page with a Content Security Policy needs these sources. `contentSecurityPolicy()` from `@whop/elements` returns every source in this table. The sandbox source applies with `environment: "sandbox"`.

| Requirement | Directive | Production source | Sandbox source |
| - | - | - | - |
| Google Pay's script | `script-src` | `https://pay.google.com` | Same |
| ↳ Google Pay's API, which pay.js calls | `connect-src` | `https://pay.google.com` | Same |
| ↳ A host Google lists for pay.js | `connect-src` | `https://google.com` | Same |
| ↳ A host Google lists for pay.js | `connect-src` | `https://account.google.com` | Same |
| ↳ A host Google lists for pay.js | `connect-src` | `https://www.google.com` | Same |
| Apple Pay's script | `script-src` | `https://applepay.cdn-apple.com` | Same |
| Basis Theory's API, which tokenizes the wallet's payment data | `connect-src` | `https://api.basistheory.com` | Same |
| Braintree's PayPal script | `script-src` | `https://js.braintreegateway.com` | Same |
| ↳ Braintree's GraphQL API, which Braintree's script calls | `connect-src` | `https://payments.braintree-api.com` | `https://payments.sandbox.braintree-api.com` |
| ↳ Braintree's client API, which Braintree's script calls | `connect-src` | `https://api.braintreegateway.com` | `https://api.sandbox.braintreegateway.com` |
| ↳ PayPal's landing frame, which Braintree's script opens | `frame-src` | `https://checkout.paypal.com` | Same |
| ↳ A Braintree frame, which Braintree's script opens | `frame-src` | `https://assets.braintreegateway.com` | Same |
| Braintree's client script, which Braintree's PayPal script loads | `script-src` | `https://assets.braintreegateway.com` | Same |
| PayPal's SDK, which Braintree's script loads | `script-src` | `https://www.paypal.com` | `https://www.sandbox.paypal.com` |
| ↳ PayPal's FraudNet script, which PayPal's SDK loads | `script-src` | `https://c.paypal.com` | Same |
| ↳ PayPal's API, which PayPal's SDK calls | `connect-src` | `https://api-m.paypal.com` | `https://api-m.sandbox.paypal.com` |
| ↳ PayPal's logger, which PayPal's SDK calls | `connect-src` | `https://www.paypal.com` | `https://www.sandbox.paypal.com` |
| ↳ PayPal's in-page window, which PayPal's SDK opens | `frame-src` | `https://www.paypal.com` | `https://www.sandbox.paypal.com` |
| ↳ PayPal's logos, which PayPal's SDK shows | `img-src` | `https://www.paypalobjects.com` | Same |

On iOS this is Apple Pay only: Google Pay is a web `show(type)` lane with no Swift counterpart. No element and no controller: an object you drive yourself, for an express checkout button above the form. `canMakePayment()` primes the sheet, and `show()` resolves to the same `ctok_` an element mints. The sheet closes with a success checkmark as soon as the customer authorizes; confirm the token on your server and show the outcome in your app.

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
            try await myBackend.confirm(token.id)
        } catch {
            // show the error in your app; the sheet has already closed
        }
    }
}
```

## Parameters

**accountID** `String` *required*

The account the charge belongs to, prefixed `biz_`.

**configuration** `WhopElementsConfiguration`

`environment`, `returnURL` and `locale` behave as they do everywhere else. Defaults to `WhopElementsConfiguration()`.

**charge** `WhopCharge` *required*

`.plan(id:)` or `.amount(minorUnits:currency:)`. Settable later; the next `canMakePayment()` re-resolves it.

**setupFutureUsage** `WhopSetupFutureUsage?`

`.offSession` or `.onSession`. Set only after showing save consent.

**options** `WhopPaymentRequest.Options`

`requestPayerEmail` and `requestBillingAddress` ask the sheet for what you did not supply (both default to `true`), and `label` overrides the line item shown above the total.

## `WhopConfirmationToken`

What a selection hands back:

* `id: String`: the `ctok_` to confirm server-side
* `paymentMethodType: WhopPaymentMethodType`: `apple_pay` on this lane

## States

`canMakePayment()` returns false when the device cannot pay, when the account has no Apple Pay merchant registered, or when the charge fails to resolve; `lastError` says which. `isPresenting` is true from `show()` until it returns or throws, including the mint after the sheet closes; a second `show()` while one is in flight throws `confirmationInFlight`. `show()` throws `walletUnavailable` rather than failing silently, and its message distinguishes a buyer who dismissed the sheet from a sheet that could not open at all; a merchant validation the API refuses throws that API error as received.

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

> [!NOTE]
>
> `WhopPaymentRequest` needs no scope and no mounted element, so it is the one payments surface that does not require `WhopBrandingElement` on screen. Show the merchant-of-record line yourself. The module is `Elements`, not the wallet SDK's `WhopElements`. See [Getting started](https://docs.whop.com/elements/latest/getting-started.md).


## Related topics

- [Payments](https://docs.whop.com/elements/latest/payments/overview.md)
- [Open a Wallet Sheet from Your Own Button](https://docs.whop.com/developer/guides/payment-request.md)
- [Void payment](https://docs.whop.com/api-reference/legacy/payments/void-payment.md)
