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

# Transfers API

> A Transfer moves money between Whop balances: from one account or user to another, from an account's wallet to a person or address, or into a claim link anyone with the URL can redeem.

Use the Transfers API to send money, find recipients you can pay, list and retrieve transfers when reconciling money movement, and preview or redeem claim links.

Subscribe to `transfer.completed` and `transfer.failed` for outcomes instead of polling. Each participating account can subscribe to these events. `transfer.created` is also emitted on success, not when processing starts. A failed transfer can be retried under the same ID and later succeed; retrieve the transfer to reconcile its current status.

A successful balance transfer credits the recipient's available balance unless a release date applies. Transfers funded from pending balance retain a release date and credit pending balance; applicable recipient reserves or fraud holds can keep funds unavailable. `succeeded` confirms the transfer completed, not that all funds are withdrawable.

## Endpoints

| Endpoint | Request |
| - | - |
| [Retrieve Claim Link](https://docs.whop.com/api-reference/transfers/retrieve-claim-link.md) | GET `/claim_links/{id}` |
| [List Transfers](https://docs.whop.com/api-reference/transfers/list-transfers.md) | GET `/transfers` |
| [Retrieve Transfer](https://docs.whop.com/api-reference/transfers/retrieve-transfer.md) | GET `/transfers/{id}` |
| [List Transfer Recipients](https://docs.whop.com/api-reference/transfers/list-transfer-recipients.md) | GET `/transfers/recipients` |
| [Claim](https://docs.whop.com/api-reference/transfers/claim.md) | POST `/claim_links/{id}/claim` |
| [Create Transfer](https://docs.whop.com/api-reference/transfers/create-transfer.md) | POST `/transfers` |

## Attributes

**id** `string` *required*

Transfer ID.

**amount** `object | null` *required*

Amount credited to the recipient. Null while the transfer has not succeeded.

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

**created\_at** `string` *required*

When the transfer was created.

**destination** `object | null` *required*

Business account or user receiving the transfer.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account or user ID.
>
> **logo\_url** `string | null` *required*
>
> Business logo or user avatar URL. Null when no image is available.
>
> **name** `string | null` *required*
>
> Business or user display name.
>
> **object** `string` *required*
>
> Whether the profile is a business account or a user.
>
> Available options: `account`, `user`

**failed\_at** `string | null` *required*

Recorded failure time. Null unless the transfer has failed.

**failure\_code** `string | null` *required*

Machine-readable failure code. Null unless the transfer has failed.

**failure\_reason** `string | null` *required*

Failure explanation. Null unless the transfer has failed.

**fee** `object | null` *required*

All applicable transfer fees, including platform markup. Null when the authoritative total is unavailable.

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

**metadata** `object` *required*

Custom metadata attached to the transfer.

**notes** `string | null` *required*

Transfer note.

**object** `string` *required*

The object type.

**origin** `object | null` *required*

Business account or user sending the transfer.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account or user ID.
>
> **logo\_url** `string | null` *required*
>
> Business logo or user avatar URL. Null when no image is available.
>
> **name** `string | null` *required*
>
> Business or user display name.
>
> **object** `string` *required*
>
> Whether the profile is a business account or a user.
>
> Available options: `account`, `user`

**status** `string` *required*

Current transfer status. A transfer can be created `processing` and settle a few seconds later; poll Retrieve Transfer or listen for the `transfer.completed` and `transfer.failed` webhooks instead of treating the create response as final. `succeeded` and `failed` are final.

Available options: `processing`, `succeeded`, `failed`

**status\_changed\_at** `string | null` *required*

Recorded time of the current transition. Null when the historical transition
time is unknown.

**succeeded\_at** `string | null` *required*

Recorded success time. Null when unknown.

**tracking\_url** `string` *required*

Tracking page for the transfer. Viewing it requires sign-in and access to the
transfer.

```json Transfer theme={null}
{
	"id": "ctt_xxxxxxxxxxxxxx",
	"object": "transfer",
	"status": "succeeded",
	"origin": {
		"id": "biz_xxxxxxxxxxxxxx",
		"object": "account",
		"name": "Shine Time Auto Detailing",
		"logo_url": null
	},
	"destination": {
		"id": "user_xxxxxxxxxxxxx",
		"object": "user",
		"name": "Dana Whitfield",
		"logo_url": null
	},
	"amount": {
		"amount": "50.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"fee": {
		"amount": "1.00",
		"currency": "usd",
		"decimals": 2,
		"display_decimals": 2
	},
	"created_at": "2026-06-01T12:00:00.000Z",
	"status_changed_at": null,
	"succeeded_at": null,
	"failed_at": null,
	"failure_code": null,
	"failure_reason": null,
	"notes": "Detailer referral bonus",
	"metadata": {
		"external_transfer_id": "transfer_123"
	},
	"tracking_url": "https://whop.com/ctt_xxxxxxxxxxxxxx/"
}
```


## Related topics

- [Transfers](https://docs.whop.com/api-reference/legacy/transfers/transfer.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
- [Transfer completed](https://docs.whop.com/api-reference/transfers/transfer-completed.md)
