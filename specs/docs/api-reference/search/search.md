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

# Search API

> Search finds one account's records in one call: members, memberships, payments, affiliates, reviews, resolution center cases, promo codes and accounts.

A pasted identifier finds its records exactly and they come first, ahead of ranked text matches.

Use the Search API behind a search box or a command menu over an account's data: find the customer behind an email or a card, open a payment from its ID or invoice number, or jump to a member by name. To filter one resource and page through every match, use that resource's list endpoint instead.

## Endpoints

| Endpoint | Request |
| - | - |
| [Search account records](https://docs.whop.com/api-reference/search/search-account-records.md) | GET `/search` |

## Who can search

Every search names one account with `account_id`, and the credential, whether an API key, OAuth token or scoped token, needs read access to at least one kind of record on it that search returns. A credential that can read none of them gets the same `404` as an account that doesn't exist.

Each row is then checked on its own account with the permission its record type needs, and rows the credential can't read are left out without an error: a credential without `payment:basic:read` on an account sees none of that account's payments. [Search account records](https://docs.whop.com/api-reference/search/search-account-records.md) lists the permission and fields of each record type. A query containing `@` also needs `member:email:read` on the row's account, and a phone number match needs `member:phone:read`.

## Subaccounts

A platform can search its subaccounts in the same call with `include_subaccounts`. Their records match by ID, email, card digits or phone number, and the subaccounts themselves match by name, ID, store link, owner email, team member email or customer email. A subaccount is also found by the ID of one of its plans or payouts. Invoice numbers and a bare last 4 only match inside the named account, because those numbers repeat across accounts.

`account` rows are `account_id` itself and, with `include_subaccounts`, its subaccounts. Every other row belongs to one of those accounts.

## How the query is read

Search trims `query`, which has to be 2 to 250 characters, and reads its shape before it runs. An identifier is looked up directly, and its matches come first. Every query of 3 or more characters is also ranked as text, and ranked rows follow the identifier matches. A query can have more than one shape: a 10-digit number is read as a phone number and as an invoice number, and each is searched.

| Shape | Example | Finds | `match_reason` |
| - | - | - | - |
| whop.com link | `whop.com/shinetime`, `https://whop.com/shinetime/products/ceramic-coating`, `/shinetime` | The named account, or with `include_subaccounts` its subaccount, that owns the store or product | `link` |
| ID | `pay_7Hk2mQ9vXb4Lp1`, `MEM_8Vt2Kp6Rq4Zn1c` | The record with that ID, in any letter case. A user ID finds the member, a dispute ID finds its payment, and a plan or payout ID finds its account | `id` |
| Email | `dana@example.com` | The members, memberships, payments and affiliates of the user with that email. With `include_subaccounts`, the subaccounts that user owns, works on or buys from. A membership with a member is found under the member's row | `email`, `owner_email`, `team_member_email`, `customer_email` |
| Card digits | `424242 4242`, `42424242-4242` | Payments on a card with that first 6 or 8 and last 4. The two parts can be separated by a space, `-`, `.`, `*`, `•`, `·` or `x` | `card` |
| Phone number | `+14155550133`, `4155550133` | Members whose user has that phone number. A bare 10-digit number is also tried as a North American number with `+1` | `phone` |
| Number | `1042`, `4242` | The named account's payment that the invoice with that number collects. A 4-digit number also finds the named account's payments on a card ending in it | `payment_number`, `card` |
| Text | `Dana Ortiz`, `SPRING20` | Ranked results only, from fields such as a customer's name, username or billing name | `name` on a whole-title match, `id` on a whole-ID match, otherwise `null` |
| Full card number | `4242 4242 4242 4242` | Nothing. Search refuses the query | None |

Ranked text search needs at least 3 characters. A shorter query only finds exact identifiers, such as a short invoice number.

A bare number is read as a phone number when it has 10 to 15 digits, and as an invoice number when it has up to 18. A longer number is only ranked.

Search never runs a full card number. A query of 13 to 19 digits that matches a card network's prefix and length and passes the card number checksum is refused with a `400`. Other long numbers fail that check, so search runs them as usual.

## Ordering

`data` comes in served order: identifier matches first, newest first within each kind of match, then ranked results, best match first. Rely on the order of `data` for ranking, and on `match_reason` for why a row matched. Each record appears once. When one type's search index is unavailable, the response is still a `200`, without that type's ranked rows.

`first` caps the number of rows: 20 by default, up to 100. There is no cursor yet, so a response is a single page.

Identifier matches read the database. Ranked matches read a search index that catches up shortly after a record changes. To filter one resource by a field such as a payment's `status`, or to page through every match, use that resource's list endpoint.

## Match reasons

`match_reason` says what the record matched on, so you can label a row without working out the reason yourself. Every identifier match has one. A ranked row has one when the query is the record's whole ID or its whole title, in any letter case; any other ranked row has a `match_reason` of `null`.

| Value | The record matched on |
| - | - |
| `id` | Its ID, or the ID of a related record: a member by its user's ID, a payment by its dispute's ID, an account by a plan or payout ID |
| `link` | A whop.com store or product link, for an account |
| `name` | Its whole title, on a ranked row |
| `email` | The email of the user it belongs to |
| `owner_email` | The email of a subaccount's owner |
| `team_member_email` | The email of someone on a subaccount's team |
| `customer_email` | The email of a subaccount's customer |
| `card` | The card's first 6 or 8 and last 4 digits, or its last 4 alone |
| `phone` | The phone number of the user it belongs to |
| `payment_number` | The number of the invoice it collects, so an invoice from `GET /invoices` comes back as its `payment` |

## Attributes

**match\_reason** `string | null` *required*

Why the row matched, or `null` for a ranked row whose whole ID or whole title isn't the query. `id`: its ID, or a related one such as its user's, its dispute's, or a plan or payout ID for an account. `link`: a whop.com store or product link. `name`: its whole title. `email`: the email of the user it belongs to. `owner_email`, `team_member_email` or `customer_email`: the email of a subaccount's owner, a member of its team, or one of its customers. `card`: its card's first 6 or 8 and last 4 digits, or the last 4 alone. `phone`: the phone number of the user it belongs to. `payment_number`: the number of the invoice a payment collects, so an invoice from `GET /invoices` comes back as its `payment`.

Available options: `id`, `owner_email`, `team_member_email`, `email`, `customer_email`, `card`, `phone`, `payment_number`, `link`, `name`

**record** `object` *required*

The matched record. Get the full record from its own endpoint by `id`.

```json SearchResult theme={null}
{
	"match_reason": "id",
	"record": {
		"id": "pay_7Hk2mQ9vXb4Lp1",
		"type": "payment",
		"title": "Marcus Webb - Ceramic Coating Masterclass",
		"subtitle": "Shine Time Detailing",
		"description": "Marcus Webb",
		"image_url": null,
		"total": {
			"currency": "usd",
			"amount": "49.00",
			"decimals": 2,
			"display_decimals": 2
		},
		"presentment_total": {
			"currency": "usd",
			"amount": "52.00",
			"decimals": 2,
			"display_decimals": 2
		},
		"status": "paid",
		"substatus": "succeeded",
		"created_at": "2026-10-12T14:03:27.418Z",
		"paid_at": "2026-10-12T14:03:29.102Z",
		"account_id": "biz_Q4nR8sVw2Ky7Lm"
	}
}
```


## Related topics

- [Search Account Records](https://docs.whop.com/api-reference/search/search-account-records.md)
- [Users API](https://docs.whop.com/api-reference/users/user.md)
- [Products API](https://docs.whop.com/api-reference/products/product.md)
