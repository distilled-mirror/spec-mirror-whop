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

# Setup Intents API

> A Setup Intent saves a buyer's payment method for later without taking money now.

Create one from a confirmation token the payment elements collected in setup mode, or from a payment method already on file to re-verify it. It runs the same collection flow a payment does, so the buyer may still owe a step: 3D Secure on a card, a hosted enrollment, or linking a bank account.

The create response is the setup intent as created, not its outcome. Hand its `client_secret` to the elements' `handleNextAction`, or poll [Retrieve status](https://docs.whop.com/api-reference/setup-intents/retrieve-setup-status.md) for how far the setup has gone and what is outstanding. Once it reaches `succeeded`, `payment_method_id` names the saved method and Create Payment charges it.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Setup Intents](https://docs.whop.com/api-reference/setup-intents/list-setup-intents.md) | GET `/setup_intents` |
| [Retrieve Setup Intent](https://docs.whop.com/api-reference/setup-intents/retrieve-setup-intent.md) | GET `/setup_intents/{id}` |
| [Retrieve setup status](https://docs.whop.com/api-reference/setup-intents/retrieve-setup-status.md) | GET `/setup_intents/{setup_intent_id}/status` |
| [Create Setup Intent](https://docs.whop.com/api-reference/setup-intents/create-setup-intent.md) | POST `/setup_intents` |
| [Update setup return URL](https://docs.whop.com/api-reference/setup-intents/update-setup-return-url.md) | PATCH `/setup_intents/{setup_intent_id}/return_url` |

## Attributes

**id** `string` *required*

Setup intent ID, prefixed `sint_`.

**account\_id** `string | null` *required*

The account the payment method is saved for, prefixed `biz_`.

**checkout\_configuration\_id** `string | null` *required*

The checkout configuration this setup was created through, prefixed `ch_`.
Null for a setup created through this API rather than a hosted checkout.

**client\_secret** `string | null` *required*

The credential a buyer's surface presents to poll this setup and set its
return URL — hand it to the elements' `handleNextAction`. Only on setups
created through this API, and always null in list responses — retrieve the
setup intent for it.

**created\_at** `string` *required*

When the setup intent was created, as an ISO 8601 timestamp.

**last\_setup\_error** `object | null` *required*

Why the setup ended where it did, or `null` when nothing has failed. Present on `canceled` — a buyer who abandoned carries no code, one refused by the provider does. Dropped once the setup succeeds.

> [accordion] Properties
>
> **code** `string | null` *required*
>
> A machine-readable classification of the failure, e.g. `enrollment_declined`.
> Absent when the buyer simply abandoned the setup.
>
> **message** `string | null` *required*
>
> A human-readable explanation of the failure.

**member\_id** `string | null` *required*

The buyer's member record on the account, prefixed `mber_`. Null without the
member:basic:read permission, unless the caller is the buyer.

**metadata** `object | null` *required*

Your own key-value data attached when the setup intent was created.

**payment\_instrument** `object | null` *required*

The method behind this setup shaped for display: a buyer-facing name, the standard icon set, and the card's brand, last four, issuer identification number, expiry, and network transaction id when it was a card. Null until a method was collected.

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

The saved payment method, prefixed `payt_`, ready to charge with Create
Payment. Null until the setup has `succeeded`.

**payment\_method\_type** `string | null` *required*

The kind of instrument being saved, for example `card` or `us_bank_account`.

**return\_url** `string | null` *required*

Where the buyer lands after completing an off-site step, or `null` to leave
them where they are.

**status** `string` *required*

How far the setup has got. **A 201 or 200 means we answered, not that the method was saved — always branch on this.** `requires_action` — the buyer has a step outstanding; hand `client_secret` to the elements or poll Retrieve setup status. `processing` — the processor is deciding. `succeeded` — the method is saved, and only this one means saved. `canceled` — abandoned or refused; see `last_setup_error`.

Available options: `processing`, `succeeded`, `canceled`, `requires_action`

**three\_ds\_level** `string | null` *required*

3D Secure behavior for supported on-session card payments. `mandate_challenge` requires a 3DS challenge before payment processing; `mandate_if_required` mandates a challenge only when the payment processor requires it; `frictionless_if_required` uses the regular frictionless 3DS flow. Payments of \$1,000 or more use `mandate_if_required` unless `mandate_challenge` is selected. Risk and authentication recovery requirements can override the preference. The setting requested for this setup, or `null` when unset.

Available options: `mandate_challenge`, `mandate_if_required`, `frictionless_if_required`

**three\_ds\_verified** `boolean` *required*

True when the buyer completed 3D Secure while saving this payment method.

**updated\_at** `string` *required*

When the setup intent was last updated, as an ISO 8601 timestamp.

**user** `object | null` *required*

The user saving the payment method. Null when the buyer is an account rather than a user.

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

```json SetupIntent theme={null}
{
	"id": "sint_xxxxxxxxxxxxxx",
	"account_id": "biz_xxxxxxxxxxxxxx",
	"checkout_configuration_id": null,
	"client_secret": null,
	"created_at": "2026-09-22T12:00:00.000Z",
	"last_setup_error": null,
	"member_id": "mber_xxxxxxxxxxxxxx",
	"metadata": {
		"customer_id": "cus_4417"
	},
	"payment_method_id": "payt_xxxxxxxxxxxxxx",
	"payment_method_type": "card",
	"return_url": "https://shinetime.example/billing/saved",
	"status": "succeeded",
	"three_ds_verified": false,
	"three_ds_level": "mandate_challenge",
	"updated_at": "2026-09-22T12:00:00.000Z",
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
		"installment_count": null,
		"payment_method_type": "card"
	}
}
```


## Related topics

- [Build a Checkout with Payment Elements](https://docs.whop.com/developer/guides/payment-elements.md)
- [Setup Intent](https://docs.whop.com/api-reference/legacy/setup-intents/setup-intent.md)
- [List setup intents](https://docs.whop.com/api-reference/legacy/setup-intents/list-setup-intents.md)
