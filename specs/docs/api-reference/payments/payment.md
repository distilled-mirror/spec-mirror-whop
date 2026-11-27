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

# Payments API

> A Payment is one charge against a buyer, for one or more variants.

Create an on-session payment with a `confirmation_token` for the method the buyer selected, or an off-session payment with an existing member's stored payment method.

Use the Payments API to charge buyers, list and retrieve payments, and refund, void, capture, or retry them. Collection runs in the background, so the create response is not the outcome. Poll [Retrieve status](https://docs.whop.com/api-reference/payments/retrieve-payment-status.md) for how far the payment has gone and, while it is `requires_action`, what the buyer must do next: follow a redirect, complete 3D Secure, display transfer instructions, or link a bank account. Update the payment's `return_url` to change where they land afterwards, up until they come back.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Payments](https://docs.whop.com/api-reference/payments/list-payments.md) | GET `/payments` |
| [Retrieve Payment](https://docs.whop.com/api-reference/payments/retrieve-payment.md) | GET `/payments/{id}` |
| [List Payment Fees](https://docs.whop.com/api-reference/payments/list-payment-fees.md) | GET `/payments/{id}/fees` |
| [Retrieve Payment Levels Rebate](https://docs.whop.com/api-reference/payments/retrieve-payment-levels-rebate.md) | GET `/payments/{payment_id}/levels_rebate` |
| [Retrieve payment status](https://docs.whop.com/api-reference/payments/retrieve-payment-status.md) | GET `/payments/{payment_id}/status` |
| [Retrieve Payment Summary](https://docs.whop.com/api-reference/payments/retrieve-payment-summary.md) | GET `/payments/summary` |
| [Create Payment](https://docs.whop.com/api-reference/payments/create-payment.md) | POST `/payments` |
| [Capture payment](https://docs.whop.com/api-reference/payments/capture-payment.md) | POST `/payments/{id}/capture` |
| [Generate Payment PDF](https://docs.whop.com/api-reference/payments/generate-payment-pdf.md) | POST `/payments/{id}/generate_pdf` |
| [Refund Payment](https://docs.whop.com/api-reference/payments/refund-payment.md) | POST `/payments/{id}/refund` |
| [Resume a parked renewal](https://docs.whop.com/api-reference/payments/resume-a-parked-renewal.md) | POST `/payments/{payment_id}/resume` |
| [Retry Payment](https://docs.whop.com/api-reference/payments/retry-payment.md) | POST `/payments/{id}/retry` |
| [Void Payment](https://docs.whop.com/api-reference/payments/void-payment.md) | POST `/payments/{id}/void` |
| [Update Payment](https://docs.whop.com/api-reference/payments/update-payment.md) | PATCH `/payments/{id}` |
| [Update payment return URL](https://docs.whop.com/api-reference/payments/update-payment-return-url.md) | PATCH `/payments/{payment_id}/return_url` |

## Attributes

**id** `string` *required*

Payment ID, prefixed `pay_`.

**account\_id** `string | null` *required*

The account that received the payment, prefixed `biz_`.

**amount\_after\_fees** `object | null` *required*

What the account keeps: the total less Whop's fees. Null when you read the payment as its buyer.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**amount\_charged** `object` *required*

What the buyer is charged for this payment: the total plus any buyer fees, in the currency the buyer pays in. Refunds do not reduce it; `presentment_refunded_amount` states them in the same currency.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**auto\_capture\_at** `string | null` *required*

When Whop will capture this authorized card payment automatically unless it is
voided first, as an ISO 8601 timestamp. Null when no automatic capture was
requested.

**auto\_refunded** `boolean` *required*

True when Whop refunded the payment automatically, for example on a dispute
alert.

**billing\_address** `object | null` *required*

The billing address the buyer entered, or null.

> [accordion] Properties
>
> **city** `string | null` *required*
>
> The city.
>
> **country** `string | null` *required*
>
> The ISO 3166-1 alpha-2 country code.
>
> **line1** `string | null` *required*
>
> The first street address line.
>
> **line2** `string | null` *required*
>
> The second street address line.
>
> **name** `string | null` *required*
>
> The name on the address.
>
> **postal\_code** `string | null` *required*
>
> The postal or ZIP code.
>
> **state** `string | null` *required*
>
> The state, province or region.

**billing\_reason** `string | null` *required*

Why the charge was created: a first purchase, a renewal, a one-time payment,
or a manual charge.

**capture\_expires\_at** `string | null` *required*

When the hold on this authorized card payment expires if it is not captured
first, as an ISO 8601 timestamp. Null when the payment was not created for
later capture.

**checkout\_configuration\_id** `string | null` *required*

The checkout configuration the buyer paid through, prefixed `ch_`, or null.

**checkout\_origin** `string | null` *required*

The scheme and host of the website where the buyer opened the checkout, such
as `https://store.example.com`. Use it to tell apart payments from different
websites on one account. Null when the website was not recorded, including for
payments made without the buyer present.

**checkout\_surface** `string | null` *required*

The checkout experience that produced this payment, such as `embedded_checkout` for a checkout on the account's own website or `off_session` for a subscription renewal. Null when the checkout was not recorded.

Available options: `hosted_checkout`, `embedded_checkout`, `custom_checkout`, `modal_checkout`, `in_app_checkout`, `mobile_checkout`, `off_session`, `card_present`

**client\_secret** `string | null` *required*

The credential a buyer's surface presents to poll this payment, set its return
URL and resume it when the bank asked the customer to authenticate. Only on
payments created from a confirmation token or waiting on that authentication,
null when you read the payment as its buyer, and always null in list responses
— retrieve the payment for it.

**created\_at** `string` *required*

When the payment was created, as an ISO 8601 timestamp.

**currency** `string` *required*

The currency the payment settles in, lowercase ISO 4217. Every money field below is stated in it unless it says otherwise.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**customer\_email** `string | null` *required*

The buyer's email address. Null without `member:email:read` on the account or
when the buyer has no assigned email.

**customer\_phone** `string | null` *required*

The phone number the buyer gave at checkout, when one was collected.

**decline\_code** `string | null` *required*

The normalized decline reason of the most recent failed attempt, or null.

**dispute\_alerted\_at** `string | null` *required*

When an issuer warned that this payment will be disputed, or null.

**dispute\_created\_at** `string | null` *required*

When the most recent dispute on this payment was opened, as an ISO 8601
timestamp. Null when the payment was never disputed, or without the
payment:dispute:read permission.

**failure\_message** `string | null` *required*

Why the most recent attempt failed, in plain words, or null.

**financing\_installments\_count** `number | null` *required*

For installment methods, how many payments the charge splits into.

**holds** `object[]` *required*

The active holds on this payment. Each hold has its own release date, independent of `settlement_time_at`. Empty when nothing is held or you read the payment as its buyer; released holds are omitted.

> [accordion] Properties
>
> **amount** `object` *required*
>
> The amount currently held, in the hold's currency.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **percentage** `number | null` *required*
>
> The reserve percentage recorded when the hold was created, for example 3.5 for
> 3.5%. Null for other hold types or when no percentage was recorded.
>
> **release\_at** `string | null` *required*
>
> When the held funds are scheduled to become available, as an ISO 8601
> timestamp. Never earlier than the payment's settlement date. Null when release
> depends on an event, such as shipment resolution, rather than a date.
>
> **type** `string` *required*
>
> The reason funds are held: `reserve`, `bnpl`, `sequra`, `fraud_hold`, or `preshipment_hold`.
>
> Available options: `reserve`, `bnpl`, `sequra`, `fraud_hold`, `preshipment_hold`

**ip\_address** `string | null` *required*

IP address that Whop captured from the buyer's browser at checkout. Null when
the buyer was not present at checkout, for example a subscription renewal or a
charge made without the buyer. Also null when you read the payment as its
buyer.

**last\_payment\_attempt\_at** `string | null` *required*

When the most recent charge attempt ran, or null.

**line\_items** `object[]` *required*

Everything this payment charged for, in purchase order, with quantities and subtotals in the purchase currency. Payments made before item snapshots were recorded return the single item implied by their variant. Empty when no items or variant can be resolved.

> [accordion] Properties
>
> **id** `string | null` *required*
>
> Line item ID, prefixed `li_`. Null when the payment predates item snapshots
> and the item is read from the payment's variant.
>
> **discount\_amount** `object | null` *required*
>
> The amount taken off this item's `subtotal` by discounts and the promo code, before tax. Returns `null` when no discount was recorded for the item.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **label** `string | null` *required*
>
> The item's name as shown at checkout (the product title, else the variant
> title).
>
> **product\_id** `string | null` *required*
>
> The product the variant belongs to, prefixed `prod_`. On a payment that
> predates item snapshots this falls back to the variant's product, so it can be
> set where the parent's own `product_id` is null. Null for a variant with no
> product.
>
> **product\_title** `string | null` *required*
>
> The product's current title, or `null` when the item has no product.
>
> **quantity** `number` *required*
>
> How many units were bought.
>
> **sku** `string | null` *required*
>
> The variant's stock keeping unit, free text set by the seller (e.g.
> `TSHIRT-LARGE-BLUE`). Accounts with billing-reason SKU suffixes enabled
> receive it stamped with why the charge happened: `-S` for a first subscription
> charge, `-R` for a renewal, `-C` for a one-time purchase, `-U` when the
> payment's own metadata carries `upsell: true`, `-F` when nothing was charged.
> Other billing reasons carry the bare SKU. `null` when the variant has no SKU
> or has been deleted.
>
> **subtotal** `object | null` *required*
>
> The recorded amount for this item's full quantity, before discounts, tax, and fees, in its purchase currency. Returns `null` when no item amount was recorded.
>
> > [accordion] Properties
> >
> > **amount** `string` *required*
> >
> > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > is ten dollars. It is a string so that no float rounds it in transit.
> >
> > **currency** `string` *required*
> >
> > Three-letter ISO 4217 currency code, lowercase.
> >
> > **decimals** `integer` *required*
> >
> > How many decimal places the amount carries. This is the precision the charge
> > itself runs at.
> >
> > **display\_decimals** `integer` *required*
> >
> > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
>
> **variant\_id** `string | null` *required*
>
> The variant bought, prefixed `plan_`. Null when the variant has since been
> deleted.
>
> **variant\_title** `string | null` *required*
>
> The variant's current title, or `null` when the variant has been deleted or has no title.

**member\_id** `string | null` *required*

The buyer's member record on the account, prefixed `mber_`. Null without the
member:basic:read permission, unless you are the buyer.

**membership\_id** `string | null` *required*

The membership this payment is billed against, prefixed `mem_`. Null for
one-off purchases, or without the member:basic:read permission unless you are
the buyer.

**metadata** `object | null` *required*

Your own key-value data attached when the payment was created. Null when you
read the payment as its buyer.

**missing\_tracking** `boolean | null` *required*

True for a paid payment with a shipping address, no shipment, and no full
refund. Null without the shipment:basic:read permission.

**needs\_tracking** `boolean | null` *required*

True when funds are held until the order ships and no tracking number has been
added yet. Null without the shipment:basic:read permission.

**next\_payment\_attempt\_at** `string | null` *required*

When the next automatic retry is scheduled, or null.

**paid\_at** `string | null` *required*

When the money was collected, or null while it has not been.

**payment\_instrument** `object | null` *required*

The instrument shaped for display: a buyer-facing name, the standard icon set, and when it was a card, the card's brand, last four, issuer identification number, and the network transaction id the account uses for the card. The network transaction id is null when you read the payment as its buyer.

> [accordion] Properties
>
> **card** `object | null` *required*
>
> Card, Apple Pay, and Google Pay payments only: the card's network, last four, issuer identification number, expiry, and network transaction id.
>
> > [accordion] Properties
> >
> > **brand** `string | null` *required*
> >
> > The network identifier (`visa`, `amex`, …), matching `card.networks` entries
> > and saved card payment methods. Null when the vault did not record the
> > network.
> >
> > **exp\_month** `number | null` *required*
> >
> > The card's expiry month, 1 to 12. Null when the vault did not record it.
> >
> > **exp\_year** `number | null` *required*
> >
> > The card's four-digit expiry year. Null when the vault did not record it.
> >
> > **issuer\_identification\_number** `string | null` *required*
> >
> > The issuer identification number, also called the BIN: the card's leading six
> > or eight digits, which identify the issuing bank. Null when the processor did
> > not report it.
> >
> > **last4** `string | null` *required*
> >
> > The card's last four digits, when captured.
> >
> > **network\_transaction\_id** `string | null` *required*
> >
> > The network transaction id this account uses for the card. It's this account's own id, from its first customer-present charge or verification on the card, when Whop has one. Otherwise it's an earlier network transaction id for this card, which can come from another account's transaction or be one brought with the card. Cite it as the prior transaction on a later merchant-initiated charge of this card at this account. Null until there is one. A webhook sent as a payment is first recorded can carry `null`, so retrieve the payment to read it.
>
> **crypto** `object | null` *required*
>
> Crypto payments only: the on-chain transaction that the buyer sent.
>
> > [accordion] Properties
> >
> > **transaction\_hash** `string | null` *required*
> >
> > The hash of the on-chain transaction that the buyer sent to pay. Use it to find the payment from a hash that the buyer gives you. Null until the payment provider reports the transaction.
>
> **display\_name** `string` *required*
>
> Buyer-facing instrument name — "Visa •••• 4242" when the card surfaced, else
> the method's own name ("Klarna").
>
> **icons** `object` *required*
>
> The standard icon set: square and card shapes, each in light and dark colorways.
>
> > [accordion] Properties
> >
> > **card** `object` *required*
> >
> > The credit-card-proportioned tile (48x30).
> >
> > > [accordion] Properties
> > >
> > > **dark** `object` *required*
> > >
> > > The colorway for dark surfaces.
> > >
> > > > [accordion] Properties
> > > >
> > > > **png\_1x** `string` *required*
> > > >
> > > > Raster fallback at the shape's native size.
> > > >
> > > > **png\_2x** `string` *required*
> > > >
> > > > Raster fallback at double density.
> > > >
> > > > **png\_4x** `string` *required*
> > > >
> > > > Raster fallback at quadruple density.
> > > >
> > > > **svg** `string` *required*
> > > >
> > > > The vector file. Prefer this everywhere SVG renders.
> > >
> > > **light** `object` *required*
> > >
> > > The colorway for light surfaces.
> > >
> > > > [accordion] Properties
> > > >
> > > > **png\_1x** `string` *required*
> > > >
> > > > Raster fallback at the shape's native size.
> > > >
> > > > **png\_2x** `string` *required*
> > > >
> > > > Raster fallback at double density.
> > > >
> > > > **png\_4x** `string` *required*
> > > >
> > > > Raster fallback at quadruple density.
> > > >
> > > > **svg** `string` *required*
> > > >
> > > > The vector file. Prefer this everywhere SVG renders.
> >
> > **square** `object` *required*
> >
> > The square tile (32x32).
> >
> > > [accordion] Properties
> > >
> > > **dark** `object` *required*
> > >
> > > The colorway for dark surfaces.
> > >
> > > > [accordion] Properties
> > > >
> > > > **png\_1x** `string` *required*
> > > >
> > > > Raster fallback at the shape's native size.
> > > >
> > > > **png\_2x** `string` *required*
> > > >
> > > > Raster fallback at double density.
> > > >
> > > > **png\_4x** `string` *required*
> > > >
> > > > Raster fallback at quadruple density.
> > > >
> > > > **svg** `string` *required*
> > > >
> > > > The vector file. Prefer this everywhere SVG renders.
> > >
> > > **light** `object` *required*
> > >
> > > The colorway for light surfaces.
> > >
> > > > [accordion] Properties
> > > >
> > > > **png\_1x** `string` *required*
> > > >
> > > > Raster fallback at the shape's native size.
> > > >
> > > > **png\_2x** `string` *required*
> > > >
> > > > Raster fallback at double density.
> > > >
> > > > **png\_4x** `string` *required*
> > > >
> > > > Raster fallback at quadruple density.
> > > >
> > > > **svg** `string` *required*
> > > >
> > > > The vector file. Prefer this everywhere SVG renders.
>
> **installment\_count** `number | null` *required*
>
> Installment methods only: how many payments the charge splits into. Data, not
> copy — compose and translate the label client-side.
>
> **payment\_method\_type** `string` *required*
>
> The payment method type identifier, e.g. `card`, `klarna`, `apple_pay`.

**payment\_method\_id** `string | null` *required*

The stored payment method that was charged, prefixed `payt_`. Null when the
method was not saved.

**payment\_method\_type** `string | null` *required*

The kind of instrument used, for example `card`, `apple_pay`, `klarna`, or
`us_bank_account`.

**payment\_rule\_matches** `object[]` *required*

The account's own payment rules that decided this payment, recorded when they ran. Only one action is taken per payment, so a rule that matched but was skipped or outranked is not listed. Empty when none decided it, when the account had no rules, when Whop blocked the payment before they ran, or when you read the payment as its buyer.

> [accordion] Properties
>
> **id** `string` *required*
>
> Payment rule ID, prefixed `prule_`.
>
> **action** `string` *required*
>
> What the rule asked for.
>
> Available options: `allow`, `block`, `review`, `enforce_3ds`
>
> **name** `string | null` *required*
>
> The rule's name when it matched. Renaming the rule afterwards does not rewrite this.

**payments\_failed** `number` *required*

How many charge attempts have failed on this payment.

**presentment\_refunded\_amount** `object | null` *required*

How much has been refunded so far, in the currency presented to the buyer. Zero when nothing has been refunded.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**presentment\_tax\_amount** `object | null` *required*

The sales tax or VAT collected, in the currency presented to the buyer. Null when no tax applied.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**presentment\_total** `object | null` *required*

The account-facing total in the currency presented to the buyer, before conversion into the settlement currency. Excludes buyer fees.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**product\_id** `string | null` *required*

The product the variant belongs to, prefixed `prod_`. Null for a variant with
no product.

**promo\_code\_id** `string | null` *required*

The promo code applied at checkout, prefixed `promo_`, or null.

**quote\_id** `string | null` *required*

The payment quote this payment charged, prefixed `pq_`: its purchase, promo
code and `tax_amount` are the quote's. Null when the payment named no quote
and tax was calculated at charge time.

**recovery\_url** `string | null` *required*

Whop-hosted URL where the buyer can sign in and complete 3D Secure for an
off-session charge the bank challenged — a subscription renewal or a
saved-card payment. Null when recovery is unavailable, you lack
`member:basic:read`, or in list responses. Retrieve the payment for it.

**refundable** `boolean` *required*

True when the payment is `paid`, not yet fully refunded, and its processor
supports refunds.

**refunded\_amount** `object | null` *required*

How much has been refunded so far, as it settled — refunds convert at the rate in force when each one was issued, not the payment's original rate.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**refunded\_at** `string | null` *required*

When the payment was refunded, or null.

**resolution\_center\_ineligible\_reason** `string | null` *required*

Why the buyer cannot open a Resolution Center case on this payment with `POST /resolution_center_cases`, or null when they can. `already_opened`: a case is already open or was decided (a withdrawn case does not count). `unsupported_payment_processor`: the payment method does not support cases. `out_of_date`: the window to open a case has passed. `not_paid`: the payment is not `paid`. `free`: nothing was charged. `already_refunded`: some of it has been refunded.

Available options: `already_opened`, `unsupported_payment_processor`, `out_of_date`, `not_paid`, `free`, `already_refunded`

**response\_due\_at** `string | null` *required*

When the earliest response is due on a dispute or Resolution Center case
awaiting the account's response, as an ISO 8601 timestamp. Null when nothing
awaits a response, or without the permission to read that dispute or case.

**retryable** `boolean` *required*

True when the payment can be retried by hand with `POST /payments/\{id}
      	/retry`. The payment must be `open` and not paid through an invoice payment
link. Its membership must be `active`, `trialing`, or `past_due`, or the
payment must be a failed first payment on a `drafted` membership. Its one
manual retry must not be used up. A retry can still be refused when the
account cannot currently accept payments.

**risk\_score** `number | null` *required*

Whop's published risk index from 0 (lowest) to 100 (highest), including
enforced decision floors. This is not a fraud probability. Null when no score
is available or you read the payment as its buyer.

**risk\_signals** `object | null` *required*

Deprecated. Risk score explanations are no longer provided; always null.
DEPRECATED: Risk score explanations are no longer provided. Always null.

**settlement\_time\_at** `string | null` *required*

When the portion not listed in `holds` posts to the account's available
balance, at midnight UTC. The `financial_activity.funds_available` webhook's
`posted_at` carries the same value when the settlement that clears it posts.
Null until the payment is paid or when you read the payment as its buyer, and
always null in list responses — retrieve the payment for it.

**shipment\_id** `string | null` *required*

The shipment fulfilling this payment, prefixed `ship_`. Null when nothing
ships or without the shipment:basic:read permission.

**shipping\_address** `object | null` *required*

The shipping address for physical goods, or null.

> [accordion] Properties
>
> **city** `string | null` *required*
>
> The city.
>
> **country** `string | null` *required*
>
> The ISO 3166-1 alpha-2 country code.
>
> **line1** `string | null` *required*
>
> The first street address line.
>
> **line2** `string | null` *required*
>
> The second street address line.
>
> **name** `string | null` *required*
>
> The name on the address.
>
> **postal\_code** `string | null` *required*
>
> The postal or ZIP code.
>
> **state** `string | null` *required*
>
> The state, province or region.

**statement\_descriptor** `string | null` *required*

The text the buyer's bank or card statement shows for this payment. Null when
the processor did not report one.

**status** `string` *required*

The lifecycle state of the charge: `open` while collection is outstanding, `paid` once the money moved, `pending` while a settlement rail clears, `void`/`uncollectible` when it ended without collecting.

Available options: `draft`, `open`, `authorized`, `paid`, `pending`, `uncollectible`, `unresolved`, `void`

**substatus** `string` *required*

The dashboard's finer-grained reading of the payment, folding in refunds, disputes and Resolution Center cases.

Available options: `succeeded`, `requires_capture`, `pending`, `failed`, `blocked`, `past_due`, `canceled`, `price_too_low`, `uncollectible`, `refunded`, `auto_refunded`, `partially_refunded`, `dispute_warning`, `dispute_needs_response`, `dispute_warning_needs_response`, `resolution_needs_response`, `dispute_under_review`, `dispute_warning_under_review`, `resolution_under_review`, `dispute_won`, `dispute_warning_closed`, `resolution_won`, `dispute_lost`, `dispute_closed`, `resolution_lost`, `drafted`, `incomplete`, `unresolved`, `open_dispute`, `open_resolution`

**subtotal** `object | null` *required*

The price before discounts, tax and fees.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**surcharge** `object | null` *required*

The card surcharge the buyer pays for paying by credit card. It is included in `total` and `amount_charged`, and appears in `line_items` as a line of its own. Null when the payment has no surcharge.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**tax\_amount** `object | null` *required*

The sales tax or VAT collected. Null when no tax applied.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**tax\_behavior** `string | null` *required*

Whether `tax_amount` was added on top of the price (`exclusive`) or was
already inside it (`inclusive`).

**tax\_refunded\_amount** `object` *required*

How much of the collected tax has been returned to the buyer so far. Zero when the payment carried no tax, or when nothing has been refunded.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**three\_ds\_verified** `boolean` *required*

True when the buyer completed 3D Secure for this payment.

**total** `object | null` *required*

The account-facing total: the price after discounts, plus any tax added on top. Excludes buyer fees, which the buyer pays above this amount — so this is not necessarily what the buyer's statement shows; `amount_charged` is what the buyer is charged.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**updated\_at** `string` *required*

When the payment last changed, as an ISO 8601 timestamp.

**usd\_total** `object | null` *required*

The total converted to USD at the time of the charge, for reporting across currencies. Excludes the adaptive pricing FX markup, which the account does not keep. Null when you read the payment as its buyer.

> [accordion] Properties
>
> **amount** `string` *required*
>
> The amount in major units, as an exact decimal string. For example, `"10.00"`
> is ten dollars. It is a string so that no float rounds it in transit.
>
> **currency** `string` *required*
>
> Three-letter ISO 4217 currency code, lowercase.
>
> **decimals** `integer` *required*
>
> How many decimal places the amount carries. This is the precision the charge
> itself runs at.
>
> **display\_decimals** `integer` *required*
>
> How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.

**user** `object | null` *required*

The buyer. Null when the buyer is an account rather than a user.

> [accordion] Properties
>
> **id** `string` *required*
>
> User ID, prefixed `user_`.
>
> **badges** `object[]` *required*
>
> Public badges for the user, in display order. Empty when the user has no badges or is suspended.
>
> > [accordion] Properties
> >
> > **type** `string` *required*
> >
> > Badge category. `whop_team` identifies a Whop team member. `verified_partner` identifies a verified member of the Whop Partners program.
> >
> > Available options: `whop_team`, `verified_partner`
>
> **name** `string | null` *required*
>
> Display name.
>
> **profile\_picture** `object` *required*
>
> Avatar wrapper. Its `url` is always present, and is a generated placeholder when the user set no picture.
>
> > [accordion] Properties
> >
> > **url** `string` *required*
> >
> > Avatar image URL. Always present. It is a generated placeholder when the user set no picture.
>
> **username** `string` *required*
>
> Public username.

**user\_agent** `string | null` *required*

The buyer's browser user agent at checkout. Null when the buyer was not
present at checkout, such as a subscription renewal, or without the
member:basic:read permission.

**variant\_id** `string | null` *required*

The variant that was charged, prefixed `plan_`.

**verification\_checks** `object | null` *required*

The Address Verification Service (AVS), cardholder name, and Card Verification Value (CVV/CVC) results, or null when the processor returned none or you read the payment as its buyer.

> [accordion] Properties
>
> **address\_line1** `string | null` *required*
>
> The Address Verification Service (AVS) result for the billing street address.
>
> **arn** `string | null` *required*
>
> The Acquirer Reference Number (ARN) that traces this charge through the card
> network, or null when the processor did not return one.
>
> **authorization\_code** `string | null` *required*
>
> The card issuer's authorization code for this charge, or null when the
> processor did not return one.
>
> **card\_holder\_name** `string | null` *required*
>
> Whether the cardholder name matched the issuer's records.
>
> **card\_security\_code** `string | null` *required*
>
> The Card Verification Value (CVV/CVC) result.
>
> **zip\_code** `string | null` *required*
>
> The Address Verification Service (AVS) result for the billing postal code.

**voidable** `boolean` *required*

True when the payment can be voided or canceled. The request is rejected if
the payment is no longer eligible — see `POST /payments/\{id}/void`.

**wuid** `string | null` *required*

The Whop pixel's visitor ID for the buyer's browser at checkout, prefixed
`wuid_`. Null when the checkout page had no Whop pixel, when the buyer was not
present at checkout, or without the member:basic:read permission.

```json Payment theme={null}
{
	"id": "pay_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"amount_after_fees": {
		"amount": "27.10",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"amount_charged": {
		"amount": "29.99",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"auto_capture_at": null,
	"auto_refunded": false,
	"billing_address": {
		"city": "Austin",
		"country": "US",
		"line1": "1114 Bouldin Ave",
		"line2": "Unit B",
		"name": "Dana Whitfield",
		"postal_code": "78704",
		"state": "TX"
	},
	"billing_reason": "subscription_create",
	"checkout_configuration_id": null,
	"capture_expires_at": null,
	"client_secret": null,
	"created_at": "2026-01-01T12:00:00.000Z",
	"currency": "usd",
	"customer_email": "marcus@shinetime.example",
	"customer_phone": "+15125550142",
	"decline_code": null,
	"dispute_alerted_at": null,
	"dispute_created_at": null,
	"failure_message": null,
	"financing_installments_count": null,
	"ip_address": "203.0.113.24",
	"last_payment_attempt_at": "2026-01-01T12:00:00.000Z",
	"line_items": [
		{
			"id": "li_xxxxxxxxxxxxxx",
			"label": "Shine Time Detailing Club",
			"variant_id": "plan_xxxxxxxxxxxxxx",
			"variant_title": "Monthly",
			"product_id": "prod_xxxxxxxxxxxxxx",
			"product_title": "Shine Time Detailing Club",
			"quantity": 1,
			"sku": "SHINE-CLUB-MONTHLY",
			"subtotal": {
				"amount": "27.99",
				"currency": "usd",
				"decimals": 2,
				"display_decimals": 2
			},
			"discount_amount": null
		}
	],
	"member_id": "mber_xxxxxxxxxxxxxx",
	"membership_id": "mem_xxxxxxxxxxxxxx",
	"metadata": {
		"order_ref": "SHINE-4417"
	},
	"missing_tracking": true,
	"needs_tracking": false,
	"next_payment_attempt_at": null,
	"paid_at": "2026-01-01T12:00:00.000Z",
	"payment_instrument": {
		"card": {
			"brand": "visa",
			"issuer_identification_number": "41111111",
			"last4": "4242",
			"exp_month": 11,
			"exp_year": 2030,
			"network_transaction_id": "016153570198200"
		},
		"crypto": null,
		"display_name": "Visa •••• 4242",
		"icons": {
			"card": {
				"dark": {
					"png_1x": "https://content.whop.com/payment_methods/visa/icons/card_dark_30.png",
					"png_2x": "https://content.whop.com/payment_methods/visa/icons/card_dark_60.png",
					"png_4x": "https://content.whop.com/payment_methods/visa/icons/card_dark_120.png",
					"svg": "https://content.whop.com/payment_methods/visa/icons/card_dark.svg"
				},
				"light": {
					"png_1x": "https://content.whop.com/payment_methods/visa/icons/card_dark_30.png",
					"png_2x": "https://content.whop.com/payment_methods/visa/icons/card_dark_60.png",
					"png_4x": "https://content.whop.com/payment_methods/visa/icons/card_dark_120.png",
					"svg": "https://content.whop.com/payment_methods/visa/icons/card_dark.svg"
				}
			},
			"square": {
				"dark": {
					"png_1x": "https://content.whop.com/payment_methods/visa/icons/card_dark_30.png",
					"png_2x": "https://content.whop.com/payment_methods/visa/icons/card_dark_60.png",
					"png_4x": "https://content.whop.com/payment_methods/visa/icons/card_dark_120.png",
					"svg": "https://content.whop.com/payment_methods/visa/icons/card_dark.svg"
				},
				"light": {
					"png_1x": "https://content.whop.com/payment_methods/visa/icons/card_dark_30.png",
					"png_2x": "https://content.whop.com/payment_methods/visa/icons/card_dark_60.png",
					"png_4x": "https://content.whop.com/payment_methods/visa/icons/card_dark_120.png",
					"svg": "https://content.whop.com/payment_methods/visa/icons/card_dark.svg"
				}
			}
		},
		"installment_count": 0,
		"payment_method_type": "card"
	},
	"payment_method_id": "payt_xxxxxxxxxxxxxx",
	"payment_method_type": "card",
	"payment_rule_matches": [
		{
			"id": "prule_xxxxxxxxxxxxx",
			"action": "allow",
			"name": "Allow returning members"
		}
	],
	"payments_failed": 0,
	"variant_id": "plan_xxxxxxxxxxxxxx",
	"presentment_refunded_amount": null,
	"presentment_tax_amount": {
		"amount": "0.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"presentment_total": {
		"amount": "29.99",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"product_id": "prod_xxxxxxxxxxxxxx",
	"promo_code_id": null,
	"recovery_url": null,
	"refundable": true,
	"refunded_amount": {
		"amount": "0.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"refunded_at": null,
	"resolution_center_ineligible_reason": null,
	"response_due_at": null,
	"retryable": false,
	"risk_score": 12,
	"risk_signals": null,
	"holds": [
		{
			"type": "reserve",
			"amount": {
				"amount": "1.00",
				"currency": "usd",
				"decimals": 2,
				"display_decimals": 2
			},
			"percentage": 3.5,
			"release_at": "2026-04-01T00:00:00.000Z"
		}
	],
	"settlement_time_at": "2026-01-03T12:00:00.000Z",
	"shipment_id": null,
	"shipping_address": {
		"city": "Austin",
		"country": "US",
		"line1": "1114 Bouldin Ave",
		"line2": "Unit B",
		"name": "Dana Whitfield",
		"postal_code": "78704",
		"state": "TX"
	},
	"statement_descriptor": "WHOP*SHINE TIME",
	"status": "paid",
	"substatus": "succeeded",
	"subtotal": {
		"amount": "27.99",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"surcharge": null,
	"tax_amount": {
		"amount": "2.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"tax_behavior": "exclusive",
	"quote_id": "pq_xxxxxxxxxxxxxx",
	"tax_refunded_amount": {
		"amount": "0.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"three_ds_verified": true,
	"total": {
		"amount": "29.99",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"updated_at": "2026-01-01T12:00:00.000Z",
	"usd_total": {
		"amount": "29.99",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"user": {
		"id": "user_xxxxxxxxxxxxxx",
		"name": "Dana Whitfield",
		"profile_picture": {
			"url": "https://ui-avatars.com/api/"
		},
		"username": "danawhitfield",
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 17_5 like Mac OS X) AppleWebKit/605.1.15",
	"verification_checks": {
		"address_line1": "PASS",
		"authorization_code": "A1B2C3",
		"arn": "74123456789012345678901",
		"card_holder_name": "PASS",
		"card_security_code": "PASS",
		"zip_code": "PASS"
	},
	"voidable": false,
	"wuid": "wuid_xxxxxxxxxxxxxx",
	"checkout_surface": "embedded_checkout",
	"checkout_origin": "https://store.example.com"
}
```


## Related topics

- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Payment Rules API](https://docs.whop.com/api-reference/payment-rules/payment-rule.md)
- [Payment Quotes API](https://docs.whop.com/api-reference/payment-quotes/payment-quote.md)
