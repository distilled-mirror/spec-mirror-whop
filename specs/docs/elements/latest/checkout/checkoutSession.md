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

# CheckoutSession

> Low-level, headless checkout session for a page that draws its own PayPal button. `load()` creates the checkout session from `lineItems`; `show("paypal", { termsAccepted: true })` sends the buyer to PayPal — a pop-up window on desktop, the whole tab on phones — and resolves with the payment once the buyer approves. With `paymentMethodOptions.paypal.requestShipping`, PayPal asks the buyer for a shipping address and the account's webhook subscribed to `shipping_address.changed` prices each one on your server while the buyer is on PayPal; the payment is created only after the buyer approves, at that price. After a full-page redirect the promise never settles; read the outcome from `redirectUrl`'s `payment` and `status` query parameters instead.

> [!NOTE]
>
> This page documents `@whop/elements@1.10.0` and `@whop/elements-react@1.10.0`.

*Since `v1.10.0`.*

Create this [`Checkout`](https://docs.whop.com/elements/latest/checkout/overview) resource without mounting an element: `whop.checkout.checkoutSession.create({ … })` in vanilla or `useWhop().checkout.checkoutSession.create({ … })` in React.

## Options

Pass these to `whop.checkout.checkoutSession.create({ … })`.

**accountId** `string` *required*

Account ID, prefixed `biz_`, that sells the line items.

**lineItems** `CheckoutSessionLineItem[]` *required*

What the buyer is buying: each entry names a variant (`plan_…`) and an optional `quantity` (default 1). Several entries form a cart.

**properties**

**variant** `string` *required*

**quantity** `number`

**redirectUrl** `string` *required*

Where the buyer lands after PayPal when the tab was redirected, with `payment` and `status` appended: `succeeded`, `canceled`, `failed`, or `processing` while Whop is still finishing the charge (read the payment from your server to see it settle). On desktop the pop-up closes instead and `show()` settles.

**metadata** `Record\<string, string\>`

Your own string keys and values, up to 40, kept on the checkout session and copied onto the payment it creates, so they come back on the payment and its webhooks, for example ad and affiliate attribution. They are set from the buyer's browser, so never rely on them for prices or fulfilment.

**setupFutureUsage** `"off_session"`

Set to `off_session` to save the buyer's PayPal account so your server can charge it later without the buyer, for example for a one-click upsell. PayPal asks the buyer to agree to future payments while they approve. Set it only after telling the buyer their PayPal account will be saved.

**email** `string`

The buyer's email, which the payment and membership belong to. Omit it to use the email of the PayPal account that approves.

**paymentMethodOptions** `CheckoutSessionPaymentMethodOptions`

Options for one payment method. `paypal.requestShipping` has PayPal ask the buyer for a shipping address. When the account has an enabled `https` webhook subscribed to `shipping_address.changed`, Whop calls it for each address and charges the totals it replies with. Defaults to `false`.

**properties**

**paypal** `CheckoutSessionPayPalOptions`

**properties**

**requestShipping** `boolean`

## Methods

Call these on the resource.

> [!WARNING]
>
> Call `show` synchronously during user activation. Await prerequisites first.

### `load`

Create the checkout session from `lineItems`. Calling it again answers the same session. `show()` loads it first when it has not been loaded. `terms` is what the buyer accepts by paying: show its `links` (the seller's published policies and Whop's buyer terms) next to your button, and when `terms.required` is `true`, have the buyer actively accept them, for example with a checkbox. `requirements` lists what the session needs before it can be paid: anything other than `email`, `terms` and, with `paypal.requestShipping`, `shipping_address` cannot be collected here, so `show()` refuses it.

**Returns** `Promise<CheckoutSessionLoaded>`

**id** `string` *required*

**status** `string` *required*

**requirements** `string[]` *required*

**terms** `CheckoutSessionTerms` *required*

**properties**

**required** `boolean` *required*

**links** `CheckoutSessionTermsLink[]` *required*

**properties**

**kind** `string` *required*

One of `"terms_of_service"`, `"privacy_policy"`, `"return_policy"`, `"eula"`, `"whop_buyer_terms"`, or `"whop_privacy_policy"`.

**url** `string` *required*

### `show`

Call it synchronously in your PayPal button's click handler: on desktop it opens the pop-up there, and a pop-up opened later is blocked. Pass `termsAccepted: true` only when `load()`'s `terms.links` were shown next to the button for this press, and, when `terms.required` is `true`, the buyer actively accepted them. Resolves with `{ payment }` once the buyer approves and Whop charges. Rejects with an error whose `code` is `attestation_required` without `termsAccepted`, `requirements_unsupported` when the session needs something PayPal cannot collect (see `load()`'s `requirements`), `payment_request_cancelled` when the buyer cancels or closes PayPal, `seller_pricing_unavailable` when the account cannot price this payment through its webhook (it does not handle its own tax, or the cart has a promo code, buyer fee, marketplace split or a plan that renews), or `payment_failed` otherwise. On phones the tab goes to PayPal and the promise never settles. Call synchronously during user activation. Await prerequisites first because browsers revoke activation across asynchronous steps.

**Parameters**

**paymentMethod** `"paypal"` *required*

**options** `CheckoutSessionShowOptions`

**properties**

**termsAccepted** `boolean`

**Returns** `Promise<CheckoutSessionConfirmResult>`

**payment** `CheckoutSessionPayment` *required*

**properties**

**id** `string` *required*

**status** `string` *required*


## Related topics

- [Checkout](https://docs.whop.com/elements/latest/checkout/overview.md)
- [Migrate from Previous Versions](https://docs.whop.com/developer/api/migrate-from-previous-versions.md)
- [Migrate from Legacy Invoices](https://docs.whop.com/developer/guides/migrate-from-legacy-invoices.md)
