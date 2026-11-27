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

# Disputes API

> A Dispute is a chargeback a customer files against a payment through their bank, or an inquiry that may become one.

It carries the disputed payment, a deadline to respond, your evidence, and the outcome once the processor rules.

Use the Disputes API to list disputes, edit the evidence packet while a dispute is still contestable, and submit it for review, or accept a chargeback you don't want to contest.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Disputes](https://docs.whop.com/api-reference/disputes/list-disputes.md) | GET `/disputes` |
| [Retrieve Dispute](https://docs.whop.com/api-reference/disputes/retrieve-dispute.md) | GET `/disputes/{id}` |
| [Retrieve Dispute Summary](https://docs.whop.com/api-reference/disputes/retrieve-dispute-summary.md) | GET `/disputes/summary` |
| [Accept Dispute](https://docs.whop.com/api-reference/disputes/accept-dispute.md) | POST `/disputes/{id}/accept` |
| [Submit Dispute](https://docs.whop.com/api-reference/disputes/submit-dispute.md) | POST `/disputes/{id}/submit` |
| [Upload Dispute Evidence](https://docs.whop.com/api-reference/disputes/upload-dispute-evidence.md) | POST `/disputes/{id}/upload_evidence` |
| [Update Dispute](https://docs.whop.com/api-reference/disputes/update-dispute.md) | PATCH `/disputes/{id}` |

## Attributes

**id** `string` *required*

Dispute ID, prefixed `dspt_`.

**acceptable** `boolean` *required*

Whether the dispute can be conceded to the customer with `POST /disputes/\{id}
      	/accept`. True only for a chargeback still awaiting a response, before
`evidence_due_at`, whose payment processor supports accepting disputes.
Inquiries are never acceptable.

**account\_id** `string | null` *required*

The account the dispute was filed against, prefixed `biz_`.

**amount** `number` *required*

The disputed amount, in whole units of `currency`.

**buyer** `object` *required*

The customer who filed the dispute.

> [accordion] Properties
>
> **email** `string | null` *required*
>
> The customer's email address. Requires the `member:email:read` scope; `null`
> without it.
>
> **member\_id** `string | null` *required*
>
> The customer's member row on the account, prefixed `mem_`.
>
> **name** `string | null` *required*
>
> The customer's display name.
>
> **user\_id** `string | null` *required*
>
> The customer's user ID, prefixed `user_`. `null` for a guest checkout.
>
> **username** `string | null` *required*
>
> The customer's Whop username.

**created\_at** `string` *required*

When the dispute was opened, as an ISO 8601 timestamp.

**currency** `string` *required*

Three-letter ISO currency code of the disputed amount.

**evidence** `object` *required*

The evidence packet sent to the processor to contest the dispute.

> [accordion] Properties
>
> **access\_activity\_log** `string | null` *required*
>
> Log of the customer's access to the product, such as sign-in or download
> activity.
>
> **billing\_address** `string | null` *required*
>
> The billing address the customer provided at checkout.
>
> **cancellation\_policy\_attachment** `object | null` *required*
>
> The cancellation policy document. Defaults to the account's cancellation policy, then its terms of service, then its return policy, then Whop's platform policy.
>
> > [accordion] Properties
> >
> > **id** `string | null` *required*
> >
> > The attachment's ID. `null` for a Whop-hosted policy, which is not an uploaded
> > file.
> >
> > **content\_type** `string | null` *required*
> >
> > The uploaded file's MIME type.
> >
> > **filename** `string | null` *required*
> >
> > The uploaded file's name.
> >
> > **platform** `boolean` *required*
> >
> > Whether this is Whop's own hosted policy, standing in because the seller
> > uploaded none. Sending it back on a PATCH changes nothing.
> >
> > **url** `string | null` *required*
> >
> > A URL to download the attachment.
>
> **cancellation\_policy\_disclosure** `string | null` *required*
>
> How the cancellation policy was shown to the customer before purchase.
>
> **customer\_communication\_attachment** `object | null` *required*
>
> Correspondence with the customer, or proof they used the product.
>
> > [accordion] Properties
> >
> > **id** `string | null` *required*
> >
> > The attachment's ID. `null` for a Whop-hosted policy, which is not an uploaded
> > file.
> >
> > **content\_type** `string | null` *required*
> >
> > The uploaded file's MIME type.
> >
> > **filename** `string | null` *required*
> >
> > The uploaded file's name.
> >
> > **platform** `boolean` *required*
> >
> > Whether this is Whop's own hosted policy, standing in because the seller
> > uploaded none. Sending it back on a PATCH changes nothing.
> >
> > **url** `string | null` *required*
> >
> > A URL to download the attachment.
>
> **customer\_email\_address** `string | null` *required*
>
> The email address the customer used at checkout.
>
> **customer\_name** `string | null` *required*
>
> The customer's name as given at checkout.
>
> **documents** `object[]` *required*
>
> Additional evidence documents, beyond the four fixed slots — set via `evidence.documents` on `PATCH /disputes/\{id}`. Each rides into the submitted packet under its `document_type`.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > The file's ID, prefixed `file_`.
> >
> > **blurhash** `string | null` *required*
> >
> > BlurHash placeholder for an image or video. Null when unavailable or removed
> > by moderation.
> >
> > **content\_type** `string | null` *required*
> >
> > The uploaded file's MIME type. Uploads are restricted to the types the processor accepts, and rejected without one — never null.
> >
> > Available options: `application/pdf`, `application/json`, `image/jpeg`, `image/png`, `image/webp`
> >
> > **created\_at** `string` *required*
> >
> > When the file was created, as an ISO 8601 timestamp.
> >
> > **document\_type** `string` *required*
> >
> > What this document proves. `return_policy`, `cancellation_policy` and `terms_of_service` are the seller's policy documents. A policy document replaces the account's copy for this dispute. `return_policy`, `cancellation_policy` and `customer_communication` also replace the matching fixed evidence slot. `shipping_policy` is the seller's shipping terms. `customer_communication` is correspondence with the buyer, such as a support thread or chat log. `product_image` is a photo of the product or service the buyer received. `physical_fulfillment` is proof that a physical order shipped and arrived. `digital_fulfillment` is proof that the buyer accessed a digital product. `customer_order_history` is the buyer's past orders with this seller. `prior_transactions` is the buyer's payment history across the platform, for a fraud defense. `customer_session` is checkout data, such as IP address, device fingerprint, AVS and CVV results, and the 3D Secure result. `subscription` is membership lifecycle evidence, such as renewals, cancellation and reminders sent. `complete_evidence` is the seller's whole response as one PDF of at most 19 pages, and it must be the only document. Whop sends a `complete_evidence` document to the processor in place of every other evidence field, attachment and document.
> >
> > Available options: `return_policy`, `shipping_policy`, `cancellation_policy`, `terms_of_service`, `physical_fulfillment`, `customer_order_history`, `product_image`, `prior_transactions`, `customer_session`, `digital_fulfillment`, `subscription`, `customer_communication`, `complete_evidence`
> >
> > **duration\_seconds** `number | null` *required*
> >
> > Audio or video duration in seconds. Null when unavailable or removed by
> > moderation.
> >
> > **filename** `string | null` *required*
> >
> > The original filename, including its extension.
> >
> > **height** `integer | null` *required*
> >
> > Image or video height in pixels. Null when unavailable or removed by
> > moderation.
> >
> > **multipart\_chunk\_size** `integer | null`
> >
> > The byte size each part (except the last) must be. Present only on create, and
> > only for multipart uploads.
> >
> > **multipart\_upload\_id** `string | null`
> >
> > The ID of the multipart upload, passed back to `complete`. Present only on
> > create, and only for multipart uploads.
> >
> > **multipart\_upload\_urls** `object[] | null`
> >
> > The presigned URL for each part. Present only on create, and only for multipart uploads.
> >
> > > [accordion] Properties
> > >
> > > **part\_number** `integer` *required*
> > >
> > > The 1-based index of this part within the multipart upload.
> > >
> > > **url** `string` *required*
> > >
> > > The presigned URL to PUT this part's bytes to.
> >
> > **object** `string` *required*
> >
> > The type of this object, always `file`.
> >
> > **optimized\_url** `string | null` *required*
> >
> > URL to the optimized file. `null` when no optimized version is available.
> >
> > **removed\_by\_moderation** `boolean` *required*
> >
> > Whether moderation removed the file. When `true`, `url` serves a replacement
> > image. `filename`, `size` and `content_type` still describe the original file.
> >
> > **size** `integer | null` *required*
> >
> > The file size in bytes. `null` until the upload has finished.
> >
> > **upload\_headers** `object`
> >
> > Headers to send with the upload PUT. Present only on create.
> >
> > **upload\_status** `string` *required*
> >
> > Where the file is in its upload lifecycle.
> >
> > Available options: `pending`, `processing`, `ready`, `failed`
> >
> > **upload\_url** `string | null`
> >
> > Presigned URL to PUT the file's bytes to. Present only on create, and only for
> > single-part uploads.
> >
> > **url** `string | null` *required*
> >
> > A URL to download the file. Public files get a permanent CDN URL, and private
> > files get a signed URL that expires. `null` until the upload has finished.
> > When moderation removes the file, a replacement image URL.
> >
> > **visibility** `string` *required*
> >
> > `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
> >
> > Available options: `public`, `private`
> >
> > **waveform\_url** `string | null` *required*
> >
> > Audio waveform URL. Null when unavailable or removed by moderation.
> >
> > **width** `integer | null` *required*
> >
> > Image or video width in pixels. Null when unavailable or removed by moderation.
>
> **notes** `string | null` *required*
>
> Any additional context for the processor reviewing the dispute.
>
> **product\_description** `string | null` *required*
>
> What the customer purchased, in the seller's own words.
>
> **refund\_policy\_attachment** `object | null` *required*
>
> The refund policy document. Defaults to the account's return policy, then its terms of service, then Whop's platform policy.
>
> > [accordion] Properties
> >
> > **id** `string | null` *required*
> >
> > The attachment's ID. `null` for a Whop-hosted policy, which is not an uploaded
> > file.
> >
> > **content\_type** `string | null` *required*
> >
> > The uploaded file's MIME type.
> >
> > **filename** `string | null` *required*
> >
> > The uploaded file's name.
> >
> > **platform** `boolean` *required*
> >
> > Whether this is Whop's own hosted policy, standing in because the seller
> > uploaded none. Sending it back on a PATCH changes nothing.
> >
> > **url** `string | null` *required*
> >
> > A URL to download the attachment.
>
> **refund\_policy\_disclosure** `string | null` *required*
>
> How the refund policy was shown to the customer before purchase.
>
> **refund\_refusal\_explanation** `string | null` *required*
>
> Why a refund was refused, when one was requested and denied.
>
> **service\_date** `string | null` *required*
>
> When the product or service was delivered.
>
> **uncategorized\_attachment** `object | null` *required*
>
> Supporting evidence that does not fit the other categories.
>
> > [accordion] Properties
> >
> > **id** `string | null` *required*
> >
> > The attachment's ID. `null` for a Whop-hosted policy, which is not an uploaded
> > file.
> >
> > **content\_type** `string | null` *required*
> >
> > The uploaded file's MIME type.
> >
> > **filename** `string | null` *required*
> >
> > The uploaded file's name.
> >
> > **platform** `boolean` *required*
> >
> > Whether this is Whop's own hosted policy, standing in because the seller
> > uploaded none. Sending it back on a PATCH changes nothing.
> >
> > **url** `string | null` *required*
> >
> > A URL to download the attachment.

**evidence\_due\_at** `string | null` *required*

The deadline to submit evidence, as an ISO 8601 timestamp. `null` when the
network already auto-resolved the dispute (Visa RDR) with no evidence round,
or when the processor hasn't reported a deadline for this dispute.

**evidence\_editable** `boolean` *required*

Whether `evidence` can still be changed and submitted.

**evidence\_locked\_reason** `string | null` *required*

Why evidence can no longer be edited. `null` while `evidence_editable` is true.

Available options: `submitted`, `response_window_closed`, `not_contestable`

**evidence\_submitted\_at** `string | null` *required*

When the evidence was submitted to the processor, as an ISO 8601 timestamp.

**fee** `object | null` *required*

The dispute fee charged to the account, in the currency it was collected in. `null` when `fee_charged` is false.

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

**fee\_charged** `boolean` *required*

Whether Whop charged the account a dispute fee for this one. `false` when the
payment was fully refunded before the dispute was opened, for a Visa RDR
resolution, and for an inquiry.

**inquiry** `boolean` *required*

Whether this is a pre-dispute inquiry rather than a formal chargeback.
Inquiries follow the same lifecycle but move no funds unless one escalates.

**issuer\_comments** `object[]` *required*

What the card issuer said when filing the dispute. Only populated when the issuer provides them, and listed in the order they were received.

> [accordion] Properties
>
> **received\_at** `string | null` *required*
>
> When the comment was received, as an ISO 8601 timestamp.
>
> **text** `string` *required*
>
> What the issuer wrote, as received.

**line\_items** `object[]` *required*

Everything the disputed payment charged for, in purchase order. `product_id` and `variant_id` name the first of these; a cart's later items appear only here. A payment made before items were recorded lists the single item its variant implies. Empty when the payment is not linked to a variant.

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

**payment** `object` *required*

The payment being disputed.

> [accordion] Properties
>
> **id** `string` *required*
>
> Payment ID, prefixed `pay_`.
>
> **amount** `number | null` *required*
>
> What the customer was charged, in whole units of the payment's currency.
>
> **card\_brand** `string | null` *required*
>
> Card brand, when the customer paid by card.
>
> **card\_last4** `string | null` *required*
>
> Last four digits of the card, when the customer paid by card.
>
> **created\_at** `string` *required*
>
> When the payment was made, as an ISO 8601 timestamp.
>
> **currency** `string | null` *required*
>
> Three-letter ISO currency code of the payment. Can differ from the dispute's
> currency when the processor settles in another currency.
>
> **payment\_instrument** `object | null` *required*
>
> The instrument this payment was made with, shaped for display: the method type, a buyer-facing name, the standard icon set, and the card facts, including the network transaction id the account uses for the card, when it was a card. Null when the payment names no method.
>
> > [accordion] Properties
> >
> > **card** `object | null` *required*
> >
> > Card, Apple Pay, and Google Pay payments only: the card's network, last four, issuer identification number, expiry, and network transaction id.
> >
> > > [accordion] Properties
> > >
> > > **brand** `string | null` *required*
> > >
> > > The network identifier (`visa`, `amex`, …), matching `card.networks` entries
> > > and saved card payment methods. Null when the vault did not record the
> > > network.
> > >
> > > **exp\_month** `number | null` *required*
> > >
> > > The card's expiry month, 1 to 12. Null when the vault did not record it.
> > >
> > > **exp\_year** `number | null` *required*
> > >
> > > The card's four-digit expiry year. Null when the vault did not record it.
> > >
> > > **issuer\_identification\_number** `string | null` *required*
> > >
> > > The issuer identification number, also called the BIN: the card's leading six
> > > or eight digits, which identify the issuing bank. Null when the processor did
> > > not report it.
> > >
> > > **last4** `string | null` *required*
> > >
> > > The card's last four digits, when captured.
> > >
> > > **network\_transaction\_id** `string | null` *required*
> > >
> > > The network transaction id this account uses for the card. It's this account's own id, from its first customer-present charge or verification on the card, when Whop has one. Otherwise it's an earlier network transaction id for this card, which can come from another account's transaction or be one brought with the card. Cite it as the prior transaction on a later merchant-initiated charge of this card at this account. Null until there is one. A webhook sent as a payment is first recorded can carry `null`, so retrieve the payment to read it.
> >
> > **crypto** `object | null` *required*
> >
> > Crypto payments only: the on-chain transaction that the buyer sent.
> >
> > > [accordion] Properties
> > >
> > > **transaction\_hash** `string | null` *required*
> > >
> > > The hash of the on-chain transaction that the buyer sent to pay. Use it to find the payment from a hash that the buyer gives you. Null until the payment provider reports the transaction.
> >
> > **display\_name** `string` *required*
> >
> > Buyer-facing instrument name — "Visa •••• 4242" when the card surfaced, else
> > the method's own name ("Klarna").
> >
> > **icons** `object` *required*
> >
> > The standard icon set: square and card shapes, each in light and dark colorways.
> >
> > > [accordion] Properties
> > >
> > > **card** `object` *required*
> > >
> > > The credit-card-proportioned tile (48x30).
> > >
> > > > [accordion] Properties
> > > >
> > > > **dark** `object` *required*
> > > >
> > > > The colorway for dark surfaces.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **png\_1x** `string` *required*
> > > > >
> > > > > Raster fallback at the shape's native size.
> > > > >
> > > > > **png\_2x** `string` *required*
> > > > >
> > > > > Raster fallback at double density.
> > > > >
> > > > > **png\_4x** `string` *required*
> > > > >
> > > > > Raster fallback at quadruple density.
> > > > >
> > > > > **svg** `string` *required*
> > > > >
> > > > > The vector file. Prefer this everywhere SVG renders.
> > > >
> > > > **light** `object` *required*
> > > >
> > > > The colorway for light surfaces.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **png\_1x** `string` *required*
> > > > >
> > > > > Raster fallback at the shape's native size.
> > > > >
> > > > > **png\_2x** `string` *required*
> > > > >
> > > > > Raster fallback at double density.
> > > > >
> > > > > **png\_4x** `string` *required*
> > > > >
> > > > > Raster fallback at quadruple density.
> > > > >
> > > > > **svg** `string` *required*
> > > > >
> > > > > The vector file. Prefer this everywhere SVG renders.
> > >
> > > **square** `object` *required*
> > >
> > > The square tile (32x32).
> > >
> > > > [accordion] Properties
> > > >
> > > > **dark** `object` *required*
> > > >
> > > > The colorway for dark surfaces.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **png\_1x** `string` *required*
> > > > >
> > > > > Raster fallback at the shape's native size.
> > > > >
> > > > > **png\_2x** `string` *required*
> > > > >
> > > > > Raster fallback at double density.
> > > > >
> > > > > **png\_4x** `string` *required*
> > > > >
> > > > > Raster fallback at quadruple density.
> > > > >
> > > > > **svg** `string` *required*
> > > > >
> > > > > The vector file. Prefer this everywhere SVG renders.
> > > >
> > > > **light** `object` *required*
> > > >
> > > > The colorway for light surfaces.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **png\_1x** `string` *required*
> > > > >
> > > > > Raster fallback at the shape's native size.
> > > > >
> > > > > **png\_2x** `string` *required*
> > > > >
> > > > > Raster fallback at double density.
> > > > >
> > > > > **png\_4x** `string` *required*
> > > > >
> > > > > Raster fallback at quadruple density.
> > > > >
> > > > > **svg** `string` *required*
> > > > >
> > > > > The vector file. Prefer this everywhere SVG renders.
> >
> > **installment\_count** `number | null` *required*
> >
> > Installment methods only: how many payments the charge splits into. Data, not
> > copy — compose and translate the label client-side.
> >
> > **payment\_method\_type** `string` *required*
> >
> > The payment method type identifier, e.g. `card`, `klarna`, `apple_pay`.
>
> **payment\_method\_type** `string | null` *required*
>
> How the customer paid, such as `card` or `paypal`.
>
> **payment\_processor** `string | null` *required*
>
> Deprecated: no longer populated. Always `null`. DEPRECATED: No longer populated. Always null.

**product\_id** `string | null` *required*

The product the disputed payment was for, prefixed `prod_`.

**reason** `string` *required*

Why the customer says they are disputing, normalized across processors and card networks. `other` covers a processor reason Whop has not categorized yet.

Available options: `fraudulent`, `unrecognized`, `declined_authorization`, `product_not_received`, `product_unacceptable`, `subscription_canceled`, `credit_not_processed`, `duplicate`, `processing_error`, `documentation_request`, `bank_cannot_process`, `other`

**reason\_code** `string | null` *required*

The raw card-network or processor reason code, such as `10.4`. Informational
only — `reason` is not derived from it.

**status** `string` *required*

`needs_response` awaits evidence (after a processor refused a filing, `submission_failure_reason` says why); `generating_response` has evidence with the processor, not yet accepted; `under_review` is with the processor; `won`/`lost` are the rulings; `prevented` settles like `lost`. `warning_` statuses are the inquiry tiers. Past `evidence_due_at` an open one reports `under_review`.

Available options: `needs_response`, `warning_needs_response`, `under_review`, `warning_under_review`, `generating_response`, `won`, `lost`, `prevented`, `warning_closed`

**submission\_failure\_reason** `string | null` *required*

Why the payment processor refused the last filing, in plain English, while the
dispute is back in `needs_response`. Fix what it names and submit again. Only
on retrieve: `dispute.updated` reports the move back to `needs_response`, then
read this here.

**updated\_at** `string` *required*

When the dispute was last changed, as an ISO 8601 timestamp.

**variant\_id** `string | null` *required*

The variant the disputed payment was made on, prefixed `plan_`.

```json Dispute theme={null}
{
	"id": "dspt_xxxxxxxxxxxxx",
	"acceptable": true,
	"account_id": "biz_xxxxxxxxxxxxxx",
	"amount": 49.99,
	"buyer": {
		"email": "dana@shinetime.example",
		"member_id": "mem_xxxxxxxxxxxxx",
		"name": "Dana Whitfield",
		"user_id": "user_xxxxxxxxxxxxx",
		"username": "danawhitfield"
	},
	"created_at": "2026-06-01T12:00:00Z",
	"currency": "usd",
	"evidence": {
		"access_activity_log": "2026-05-02 14:11 UTC signed in and opened the course; 2026-05-04 09:32 UTC downloaded lesson materials",
		"billing_address": "123 Main St, Austin, TX 78701, US",
		"cancellation_policy_attachment": null,
		"cancellation_policy_disclosure": "Shown on the checkout page beneath the pay button.",
		"customer_communication_attachment": {
			"id": "att_xxxxxxxxxxxxx",
			"content_type": "application/pdf",
			"filename": "support-thread.pdf",
			"platform": false,
			"url": "https://assets.whop.com/uploads/att_xxxxxxxxxxxxx/support-thread.pdf"
		},
		"customer_email_address": "dana@shinetime.example",
		"customer_name": "Dana Whitfield",
		"notes": "The ceramic coating was completed and the vehicle collected before the dispute.",
		"product_description": "Monthly maintenance washes, interior vacuuming, and a quarterly sealant top-up.",
		"refund_policy_attachment": {
			"id": null,
			"content_type": "application/pdf",
			"filename": "refund-policy.pdf",
			"platform": true,
			"url": "https://whop.com/policies/refund-policy.pdf"
		},
		"refund_policy_disclosure": "Linked in the order confirmation email and on the checkout page.",
		"refund_refusal_explanation": "Refund was refused because the membership had been used past the 7-day refund window.",
		"service_date": "2026-05-01",
		"uncategorized_attachment": null,
		"documents": [
			{
				"id": "file_xxxxxxxxxxxxxx",
				"object": "file",
				"document_type": "product_image",
				"filename": "before-and-after-coating.png",
				"content_type": "image/png",
				"size": 48213,
				"url": "https://cdn.whop.com/attachments/before-and-after-coating.png",
				"optimized_url": null,
				"upload_status": "ready",
				"visibility": "private",
				"created_at": "2026-06-01T12:00:00Z",
				"width": null,
				"height": null,
				"blurhash": null,
				"duration_seconds": null,
				"waveform_url": null,
				"removed_by_moderation": false
			}
		]
	},
	"evidence_due_at": "2026-06-15T00:00:00Z",
	"evidence_editable": true,
	"evidence_locked_reason": null,
	"submission_failure_reason": null,
	"evidence_submitted_at": null,
	"fee": {
		"amount": "15.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"fee_charged": true,
	"inquiry": false,
	"issuer_comments": [
		{
			"received_at": "2026-06-01T12:00:00Z",
			"text": "Cardholder states they did not authorize this transaction."
		}
	],
	"payment": {
		"id": "pay_xxxxxxxxxxxxx",
		"amount": 49.99,
		"card_brand": "visa",
		"card_last4": "4242",
		"created_at": "2026-05-01T09:30:00Z",
		"currency": "usd",
		"payment_method_type": "card",
		"payment_processor": null,
		"payment_instrument": {
			"payment_method_type": "card",
			"display_name": "Visa •••• 4242",
			"icons": {
				"square": {
					"light": {
						"svg": "https://content.whop.com/payment_methods/visa/square_light.svg",
						"png_1x": "https://content.whop.com/payment_methods/visa/square_light_32.png",
						"png_2x": "https://content.whop.com/payment_methods/visa/square_light_64.png",
						"png_4x": "https://content.whop.com/payment_methods/visa/square_light_128.png"
					},
					"dark": {
						"svg": "https://content.whop.com/payment_methods/visa/square_dark.svg",
						"png_1x": "https://content.whop.com/payment_methods/visa/square_dark_32.png",
						"png_2x": "https://content.whop.com/payment_methods/visa/square_dark_64.png",
						"png_4x": "https://content.whop.com/payment_methods/visa/square_dark_128.png"
					}
				},
				"card": {
					"light": {
						"svg": "https://content.whop.com/payment_methods/visa/card_light.svg",
						"png_1x": "https://content.whop.com/payment_methods/visa/card_light_30.png",
						"png_2x": "https://content.whop.com/payment_methods/visa/card_light_60.png",
						"png_4x": "https://content.whop.com/payment_methods/visa/card_light_120.png"
					},
					"dark": {
						"svg": "https://content.whop.com/payment_methods/visa/card_dark.svg",
						"png_1x": "https://content.whop.com/payment_methods/visa/card_dark_30.png",
						"png_2x": "https://content.whop.com/payment_methods/visa/card_dark_60.png",
						"png_4x": "https://content.whop.com/payment_methods/visa/card_dark_120.png"
					}
				}
			},
			"card": {
				"brand": "visa",
				"last4": "4242",
				"issuer_identification_number": "41111111",
				"exp_month": 11,
				"exp_year": 2030,
				"network_transaction_id": "016153570198200"
			},
			"crypto": null,
			"installment_count": null
		}
	},
	"line_items": [
		{
			"id": "li_xxxxxxxxxxxxx",
			"label": "Ceramic Coating Package",
			"variant_id": "plan_xxxxxxxxxxxxx",
			"variant_title": "One-time",
			"product_id": "prod_xxxxxxxxxxxxx",
			"product_title": "Ceramic Coating Package",
			"quantity": 1,
			"sku": "CERAMIC-COAT-3YR",
			"subtotal": {
				"amount": "299.00",
				"currency": "usd",
				"decimals": 2,
				"display_decimals": 2
			},
			"discount_amount": null
		}
	],
	"variant_id": "plan_xxxxxxxxxxxxx",
	"product_id": "prod_xxxxxxxxxxxxx",
	"reason": "fraudulent",
	"reason_code": "10.4",
	"status": "needs_response",
	"updated_at": "2026-06-01T12:00:00Z"
}
```


## Related topics

- [Refunds and Disputes](https://docs.whop.com/developer/guides/refunds-and-disputes.md)
- [Dispute Alerts API](https://docs.whop.com/api-reference/dispute-alerts/dispute-alert.md)
- [Dispute](https://docs.whop.com/api-reference/legacy/disputes/dispute.md)
