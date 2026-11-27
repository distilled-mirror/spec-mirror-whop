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

# Cards API

> Cards represent virtual payment cards that spend from an account or user balance.

Cards can be assigned to cardholders and configured with spending limits for controlled spending.

Use the Cards API to issue cards, list cards for an account or user, retrieve active card details such as the card number and CVC, and list the transactions made with them.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Card Transactions](https://docs.whop.com/api-reference/cards/list-card-transactions.md) | GET `/card_transactions` |
| [Retrieve Card Transaction](https://docs.whop.com/api-reference/cards/retrieve-card-transaction.md) | GET `/card_transactions/{id}` |
| [List Cards](https://docs.whop.com/api-reference/cards/list-cards.md) | GET `/cards` |
| [Retrieve Card](https://docs.whop.com/api-reference/cards/retrieve-card.md) | GET `/cards/{id}` |
| [Create Card](https://docs.whop.com/api-reference/cards/create-card.md) | POST `/cards` |
| [Update Card](https://docs.whop.com/api-reference/cards/update-card.md) | PATCH `/cards/{id}` |

## Attributes

**id** `string` *required*

Card ID, prefixed `icrd_`.

**billing** `object | null` *required*

The billing address.

> [accordion] Properties
>
> **city** `string | null` *required*
>
> Billing city.
>
> **country\_code** `string | null` *required*
>
> Billing country code.
>
> **line1** `string | null` *required*
>
> Street address line 1.
>
> **line2** `string | null` *required*
>
> Street address line 2.
>
> **postal\_code** `string | null` *required*
>
> Billing postal code.
>
> **region** `string | null` *required*
>
> Billing region or state.

**canceled\_at** `string | null` *required*

When the card was canceled.

**created\_at** `string | null` *required*

When the card was created.

**digital\_wallets** `string[] | null` *required*

The digital wallets the card has been added to. List Cards, Update Card, and webhook payloads return `null`, as does Retrieve Card when the wallets cannot be retrieved.

Available options: `apple_pay`, `google_pay`

**expiration\_month** `string | null` *required*

Card expiration month.

**expiration\_year** `string | null` *required*

Card expiration year.

**issued** `boolean` *required*

`true` once the issuer has issued the card. `false` for an invited card still
waiting on its cardholder's onboarding or review, or one the issuer declined;
it has no card number until it is issued.

**last4** `string | null` *required*

Last four digits of the card number. `null` for pending invitation cards.

**limit** `object | null` *required*

The spending limit configuration.

> [accordion] Properties
>
> **amount** `number` *required*
>
> The limit amount in dollars.
>
> **frequency** `string` *required*
>
> The window the limit amount applies to. `per_transaction` caps each individual authorization and is what a limit set with `transaction_limit` reports.
>
> Available options: `daily`, `weekly`, `monthly`, `one_time`, `per_transaction`

**name** `string | null` *required*

Card display name.

**object** `string` *required*

**secrets** `object | null`

Sensitive card details. Present only on `GET /cards/:id` for active cards; `null` when the card is inactive or details cannot be retrieved.

> [accordion] Properties
>
> **card\_number** `string` *required*
>
> Full card number.
>
> **cvc** `string` *required*
>
> Card verification code.
>
> **name\_on\_card** `string | null` *required*
>
> Cardholder name printed on the card.
>
> **pin** `string | null` *required*
>
> The card PIN. Only returned when the request is authenticated as the user the card is assigned to; `null` for all other callers, including account API keys.

**spent\_last\_month** `integer | null` *required*

Total spend in the last 30 days, in cents.

**status** `string | null` *required*

The card status. `denied` means the issuer declined the cardholder, so the card will never be issued.

Available options: `active`, `frozen`, `canceled`, `invited`, `denied`

**type** `string | null` *required*

The card type.

Available options: `virtual`, `physical`

**user\_id** `string | null` *required*

Cardholder user ID, prefixed `user_`, when assigned.

```json Card theme={null}
{
	"id": "icrd_xxxxxxxxxxxxx",
	"billing": {
		"city": "New York",
		"country_code": "US",
		"line1": "123 Spring Street",
		"line2": "Suite 4",
		"postal_code": "10012",
		"region": "NY"
	},
	"canceled_at": null,
	"created_at": "2026-06-01T12:00:00Z",
	"digital_wallets": ["apple_pay"],
	"expiration_month": "12",
	"expiration_year": "2029",
	"issued": true,
	"last4": "4242",
	"limit": {
		"amount": 500,
		"frequency": "daily"
	},
	"name": "Operations Card",
	"object": "card",
	"secrets": {
		"card_number": "4242424242424242",
		"cvc": "123",
		"name_on_card": "Shine Time Auto Detailing Operations",
		"pin": "1234"
	},
	"spent_last_month": 12500,
	"status": "active",
	"type": "virtual",
	"user_id": "user_xxxxxxxxxxxxx"
}
```


## Related topics

- [Card canceled](https://docs.whop.com/api-reference/cards/card-canceled.md)
- [Card created](https://docs.whop.com/api-reference/cards/card-created.md)
- [Card frozen](https://docs.whop.com/api-reference/cards/card-frozen.md)
