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

# Checkout

> Drives a full hosted checkout for one or more plans — itemized price summary, promo codes, the currency the buyer pays in, and the whole payment collection surface (the payments elements, composed inside) — against the Whop checkout sessions API. Mount it with `items`, a single `plan`, or a `checkoutConfiguration` you authored and the element opens the checkout session itself; the session credential never leaves the element. A setup-mode checkout configuration mounts the same element as a payment-method save: nothing is charged, the buyer’s method is stored for you to charge later, and the finished checkout redirects with `setup_intent_id`. The buyer pays inside the element, and a finished checkout redirects the current tab to `returnUrl`, including the page that contains the element. Fulfill from webhooks rather than a browser callback. Without a `returnUrl`, the buyer rests on the element’s own success face. The element automatically drives an off-site payment step, such as 3DS or a bank page. On whop.com, it brings the buyer back into the restored checkout; in an embed, the buyer returns to the same `returnUrl` whatever the outcome, with `payment` and `status` query parameters (`succeeded`, `failed`, or `canceled`), so the page at `returnUrl` must read `status` before it treats the visit as a sale. A payment that fails inside the element reopens the same checkout with the reason shown, so the buyer can pay again. Every option is set at creation: the element mints a checkout session from these values when it mounts. Changing one later with `update()` or new React props fails instead of changing the existing order. Mount a new checkout to change what is being purchased.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.0.0`.*

## Playground

Assemble the elements with example data. Drive the controls, add and arrange elements, and watch events fire live:

```tsx React theme={null}
import { WhopElements, Checkout } from "@whop/elements-react";
import { loadWhop } from "@whop/elements";

function Example() {
  return (
    <WhopElements elements={loadWhop()}>
      <Checkout /* options */>
        {/* mount elements here */}
      </Checkout>
    </WhopElements>
  );
}
```

```html JavaScript theme={null}
<script src="https://cdn.whop.com/elements/amber/elements.js" data-whop-elements></script>
<script type="module">
  const checkout = window.WhopElements().checkout.create({ /* options */ });
</script>
```

## Options

Pass these to `whop.checkout.create({ … })`, or as props on `<Checkout>` in React.

**plan** `string`

Existing plan ID, prefixed `plan_`. Shorthand for an `items` array containing this plan and `quantity`. The plan defines the price; nothing about amounts can be asserted client-side. Pass either `plan`, `items`, or a `checkoutConfiguration`. Set at create only: the session is minted from it, and changing it later refuses.

**quantity** `number`

How many units to purchase. Plans that disallow multiples refuse values above 1. Set at create only: the session is minted with it, and changing it later refuses. @default 1

**items** `CheckoutItemInput[]`

The plans in this checkout and how many units of each to purchase. Every item must belong to the same seller and use compatible billing terms; the API validates the complete cart and prices it as one charge. Pass this instead of `plan`, `quantity`, or `checkoutConfiguration`. Set at create only: the session is minted from this exact cart, and changing it later refuses.

**properties**

**plan** `string` *required*

Existing plan ID, prefixed `plan_`.

**quantity** `number`

Number of units to purchase. Defaults to 1; plans can disallow quantities above 1.

**checkoutConfiguration** `string`

Existing checkout configuration ID, prefixed `ch_`. The element creates the checkout session from it: the configuration names the plan being sold (or, in `setup` mode, no plan — the checkout saves a payment method and charges nothing) and seeds its own affiliate code, metadata, redirect and payment-method presets — an option passed here beside it wins over the configuration’s copy, field by field. Pass `plan` alongside only to set `quantity`; it must be the configuration’s own plan, which can never be swapped out from under its presets. Set at create only.

**promoCode** `string`

A promo code to apply before the buyer ever sees the price — the discount is on the first quote, and the buyer can still change or remove it. Pass the code a marketing link carried: a code that has expired or was never valid opens the checkout at full price with the reason on the promo row, never a checkout that will not open. Set at create only.

**affiliateCode** `string`

The affiliate this checkout is attributed to. Set at create only.

**attribution** `CheckoutAttributionInput`

Where this sale came from, for the account's reporting: the campaign parameters of the link the buyer arrived on (`utmSource`, `utmMedium`, `utmCampaign`, `utmTerm`, `utmContent`, any other `utm_*` parameter under `utm`, a Whop `trackingLinkId`, a `funnelId`, and your own `source`). Everything here is recorded against the order and is never shown to the buyer. Set at create only.

**properties**

**utmSource** `string`

**utmMedium** `string`

**utmCampaign** `string`

**utmTerm** `string`

**utmContent** `string`

**trackingLinkId** `string`

**funnelId** `string`

**source** `string`

**utm** `Record\<string, string\>`

Any other `utm_*` parameter the link carried, keyed by its full name (`{ utm_funnel: 'spring' }`). Keys that do not start with `utm_` are ignored, and the named fields above win over the same key here.

**metadata** `Record\<string, string\>`

Your own key/value pairs to record against the order and read back on the payment — order IDs, cart IDs, or anything you reconcile against. Up to 40 string pairs. Set at create only.

**returnUrl** `string`

Where the buyer lands after an off-site payment step, whatever its outcome. The tab arrives with `payment` (the `pay_` ID; `setup` with the `sint_` ID for a setup-mode checkout) and `status` (`succeeded`, `failed`, or `canceled`) appended, and your other query parameters intact. Branch on `status` before showing a success page: a declined or abandoned off-site step lands here too. Fulfill from webhooks, not from this visit. Https-only (http for localhost). Set at create only.

**appearance** `Appearance`

Visual customization for this group's elements. Overrides the global `WhopElements({ appearance })`. Change it live with `update({ appearance })`.

**locale** `WhopElementsLocale`

Locale for this group's element UI text. Set it to one of the app's built locales to override the global configuration. Any other value falls back to the default locale.

## Events

Pass callbacks in the create options or React props.

### `onComplete`

Fires once per mount when the checkout completes and its result stands. `result` says what stands: `payment` (the buyer paid — `paymentId`), `waitlist_entry` (the buyer joined a waitlist — `entryId`) or `setup` (a setup-mode checkout saved the payment method — `setupIntentId`); `sessionId` is the checkout session either way. Fires before any `returnUrl` navigation, so this is where your own analytics or ad pixels record the purchase. Fulfill from webhooks, not from this callback: a checkout restored on a later page load fires it again for the same result.

**payload** `CheckoutPaymentCompletion | CheckoutWaitlistCompletion | CheckoutSetupCompletion`

**CheckoutPaymentCompletion**

**sessionId** `string` *required*

The checkout session, prefixed `chs_`.

**result** `"payment"` *required*

**paymentId** `string` *required*

The payment, prefixed `pay_` — the id to record the purchase under.

**CheckoutWaitlistCompletion**

**sessionId** `string` *required*

The checkout session, prefixed `chs_`.

**result** `"waitlist_entry"` *required*

**entryId** `string` *required*

The waitlist entry, prefixed `ent_`.

**CheckoutSetupCompletion**

**sessionId** `string` *required*

The checkout session, prefixed `chs_`.

**result** `"setup"` *required*

**setupIntentId** `string` *required*

The saved setup, prefixed `sint_`.

### `onLoadingChange`

Runs when the grouped loading state changes. The value is `true` while any mounted element is still loading.

**loading** `boolean`

## Methods

Call these on the Checkout handle from `whop.checkout.create({ … })` or `useCheckout()`.

### `update`

Merges new handle options into every mounted element. In React, change the namespace props instead.

**Parameters**

**options** `Partial\<CheckoutOptions\>` *required*

### `destroy`

Destroys every element and sub-controller this handle created, removes the controller frame, and releases its subscriptions. You can call it more than once, but a destroyed handle refuses any other call — create a new handle to start over. React removes the group automatically when the provider unmounts.

## Resources

Create these SDK objects without mounting an element. Each has its own page:

> [card] [CheckoutSession](https://docs.whop.com/elements/latest/checkout/checkoutSession.md)
>
> Low-level, headless checkout session for a page that draws its own PayPal button. `load()` creates the checkout session from `lineItems`; `show("paypal", { termsAccepted: true })` sends the buyer to PayPal — a pop-up window on desktop, the whole tab on phones — and resolves with the payment once the buyer approves. With `paymentMethodOptions.paypal.requestShipping`, PayPal asks the buyer for a shipping address and the account's webhook subscribed to `shipping_address.changed` prices each one on your server while the buyer is on PayPal; the payment is created only after the buyer approves, at that price. After a full-page redirect the promise never settles; read the outcome from `redirectUrl`'s `payment` and `status` query parameters instead.

## Elements

The elements this group mounts. Each has its own page:

**Alternatives.** Mount one at a time. Destroy it before mounting another:

> [card] [CheckoutElement](https://docs.whop.com/elements/latest/checkout/checkout.md)
>
> The full checkout surface — order summary with the live quote, promo code entry, the currency the buyer pays in, everything this checkout has to collect from the buyer, the composed payment methods surface, and the pay flow. What it collects is whatever the seller set up (email, their own questions, a phone number, a shipping address for physical goods, an explicit agreement to their terms), rendered together on one screen in the order they chose — so a seller who starts asking for something new needs no change here. Renders the whole purchase; the checkout controller owns the session and the credential. After a payment that needs an off-site step, this same surface renders the outcome the server reports when the buyer comes back.

> [card] [ExpressCheckoutElement](https://docs.whop.com/elements/latest/checkout/expressCheckout.md)
>
> One-press Apple Pay, Google Pay and PayPal buttons for a checkout — the wallet collects whatever the session still needs (the buyer's email, a shipping address for physical goods, a phone number where the seller collects one in Apple Pay and Google Pay, and in Apple Pay a promo code where the seller offers entry) and one press finishes the purchase. It shares the checkout's entry slot with the full checkout element, so a handle mounts exactly one of the two: this button alone where the purchase is simple enough for a sheet to finish, or the full checkout surface for everything else. Renders only the wallets the buyer's device can actually pay with — and the checkout's own payment method configuration allows — using the served button art, and renders nothing where no wallet is available. PayPal appears where the seller accepts it, opens in its own window, and saves the buyer's PayPal account with the purchase, so a renewal, a free trial and later charges the seller makes work like a saved card. All three wallets require Apple Pay on the page's domain: first-party whop.com pages are pre-approved, and any other site must add Apple Pay to its domain through the [Domains API](https://docs.whop.com/api-reference/beta/domains/domain) before any of these buttons renders there. Offering Google Pay on your own site is subject to the Google Pay API Terms of Service. A checkout the sheet cannot honestly finish — a waitlist join, a transfer, a form question only a page can ask — refuses loudly instead of rendering a doomed button.


## Related topics

- [Checkout Configuration](https://docs.whop.com/api-reference/legacy/checkout-configurations/checkout-configuration.md)
- [Embed a Checkout](https://docs.whop.com/developer/guides/embed-checkout.md)
- [Checkout Configurations API](https://docs.whop.com/api-reference/checkout-configurations/checkout-configuration.md)
