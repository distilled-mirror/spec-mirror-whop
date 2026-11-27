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

# Confirmation Tokens API

> A Confirmation Token is a single-use, short-lived reference to a payment method and billing details collected from a buyer.

Its response never returns the underlying payment credential.

Whop Elements mint the token in your buyer-facing collection flow and hand you its `ctok_` ID to send to the Payments API from your server. Retrieve a token to display its payment method and billing preview or check whether it is still usable.

## Endpoints

| Endpoint | Request |
| - | - |
| [Retrieve Confirmation Token](https://docs.whop.com/api-reference/confirmation-tokens/retrieve-confirmation-token.md) | GET `/confirmation_tokens/{id}` |

## Attributes

**id** `string` *required*

**billing\_address** `object | null` *required*

The collected billing address, including the name on the address. Null when not collected or without bearer authentication with payment:basic:read on the token’s account.

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

**billing\_details** `object | null` *required*

Billing preview supplied at collection: email, name, country and postal code.

> [accordion] Properties
>
> **country** `string | null` *required*
>
> ISO 3166-1 alpha-2 country code.
>
> **email** `string | null` *required*
>
> Email supplied when the method was collected.
>
> **name** `string | null` *required*
>
> Name on the payment method.
>
> **postal\_code** `string | null` *required*
>
> Postal or ZIP code.

**created\_at** `string` *required*

When the token was created, as an ISO 8601 timestamp.

**expires\_at** `string` *required*

When the token expires, as an ISO 8601 timestamp. Tokens are single-use and
short-lived.

**object** `string` *required*

Always `confirmation_token`.

**payment\_method\_preview** `object` *required*

Display-only preview of the collected method — never the underlying token.

> [accordion] Properties
>
> **id** `string | null` *required*
>
> The saved payment method this preview came from, or `null` when the buyer
> supplied a new one.
>
> **bank\_debit** `object`
>
> Present when the category is `bank_debit`. Empty until the account is charged.
>
> > [accordion] Properties
> >
> > **brand** `string`
> >
> > Lowercase card brand, such as `visa` or `mastercard`.
> >
> > **fingerprint** `string`
> >
> > Uniquely identifies this particular card number. Matches the `fingerprint` on
> > any payment method saved from this token, so you can recognize a card across
> > attempts. For a wallet, this identifies the network token rather than the
> > underlying card.
> >
> > **last4** `string`
> >
> > The last four digits of the card.
>
> **card** `object`
>
> Details of the card, when the category is `card`.
>
> > [accordion] Properties
> >
> > **brand** `string`
> >
> > Lowercase card brand, such as `visa` or `mastercard`.
> >
> > **fingerprint** `string`
> >
> > Uniquely identifies this particular card number. Matches the `fingerprint` on
> > any payment method saved from this token, so you can recognize a card across
> > attempts. For a wallet, this identifies the network token rather than the
> > underlying card.
> >
> > **last4** `string`
> >
> > The last four digits of the card.
>
> **category** `string` *required*
>
> The family the type belongs to.
>
> Available options: `card`, `wallet`, `bank_debit`, `bank_transfer`, `voucher`, `redirect`, `crypto`, `balance`, `in_app_purchase`, `card_present`, `saved`
>
> **display\_name** `string` *required*
>
> Human-readable label for the method, e.g. `Visa •••• 4242`.
>
> **saved** `object`
>
> Details of the stored card, when the category is `saved`. Absent for a balance.
>
> > [accordion] Properties
> >
> > **brand** `string`
> >
> > Lowercase card brand, such as `visa` or `mastercard`.
> >
> > **fingerprint** `string`
> >
> > Uniquely identifies this particular card number. Matches the `fingerprint` on
> > any payment method saved from this token, so you can recognize a card across
> > attempts. For a wallet, this identifies the network token rather than the
> > underlying card.
> >
> > **last4** `string`
> >
> > The last four digits of the card.
>
> **type** `string` *required*
>
> The payment method type, e.g. `card`, `apple_pay`, `klarna`.
>
> **wallet** `object`
>
> Details of the network token the wallet supplied, when the category is `wallet`.
>
> > [accordion] Properties
> >
> > **brand** `string`
> >
> > Lowercase card brand, such as `visa` or `mastercard`.
> >
> > **fingerprint** `string`
> >
> > Uniquely identifies this particular card number. Matches the `fingerprint` on
> > any payment method saved from this token, so you can recognize a card across
> > attempts. For a wallet, this identifies the network token rather than the
> > underlying card.
> >
> > **last4** `string`
> >
> > The last four digits of the card.

**setup\_future\_usage** `string | null` *required*

Save-consent state the element displayed at collection: `off_session`,
`on_session`, or `null`. Confirm may vault only if attested here.

**status** `string` *required*

`pending` until it is used, then `consumed`; `expired` once its short lifetime elapses. Only a `pending` token can be charged.

Available options: `pending`, `consumed`, `expired`

```json ConfirmationToken theme={null}
{
	"id": "ctok_xxxxxxxxxxxxxx",
	"object": "confirmation_token",
	"status": "pending",
	"payment_method_preview": {
		"id": null,
		"type": "card",
		"category": "card",
		"display_name": "Visa •••• 4242",
		"card": {
			"brand": "visa",
			"fingerprint": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
			"last4": "4242"
		}
	},
	"setup_future_usage": "off_session",
	"billing_details": {
		"email": "dana@shinetime.example",
		"name": "Dana Whitfield",
		"country": "US",
		"postal_code": "78701"
	},
	"billing_address": null,
	"created_at": "2026-08-28T12:00:00.000Z",
	"expires_at": "2026-08-28T12:15:00.000Z"
}
```


## Related topics

- [Retrieve Confirmation Token](https://docs.whop.com/api-reference/confirmation-tokens/retrieve-confirmation-token.md)
- [Build a Checkout with Payment Elements](https://docs.whop.com/developer/guides/payment-elements.md)
- [Open a Wallet Sheet from Your Own Button](https://docs.whop.com/developer/guides/payment-request.md)
