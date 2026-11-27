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

# Ledger Account

> A ledger account represents a financial account on Whop that can hold many balances.

```json Example theme={null}
{
	"balances": [
		{
			"balance": 6.9,
			"currency": "usd",
			"pending_balance": 6.9,
			"reserve_balance": 6.9
		}
	],
	"id": "ldgr_xxxxxxxxxxxxx",
	"ledger_type": "primary",
	"owner": {
		"id": "user_xxxxxxxxxxxxx",
		"name": "Marcus Webb",
		"typename": "User",
		"username": "marcuswebb"
	},
	"payments_approval_status": "pending",
	"payout_account_details": {
		"address": {
			"city": "<string>",
			"country": "<string>",
			"line1": "<string>",
			"line2": "<string>",
			"postal_code": "<string>",
			"state": "<string>"
		},
		"business_name": "<string>",
		"business_representative": {
			"date_of_birth": "<string>",
			"first_name": "<string>",
			"last_name": "<string>",
			"middle_name": "<string>"
		},
		"email": "<string>",
		"id": "poact_xxxxxxxxxxxx",
		"latest_verification": {
			"id": "verf_xxxxxxxxxxxxx",
			"last_error_code": "abandoned",
			"last_error_reason": "Document image was too blurry to read.",
			"status": "requires_input"
		},
		"phone": "<string>",
		"status": "connected"
	},
	"payout_quote_required": true,
	"settlement_time_at": "2023-12-01T05:00:00.401Z",
	"transfer_fee": 6.9,
	"treasury_balance": {
		"balance": 6.9,
		"balance_usd": 6.9,
		"currency": "usd",
		"pending_balance": 6.9,
		"reserve_balance": 6.9,
		"total_withdrawable_balance": 6.9
	}
}
```

**balances** `array\<object\>` *required*

The balances associated with the account.

**child attributes**

**balance** `number` *required*

The amount of the balance.

Example: `6.9`

**currency** `Currencies` *required*

The currency of the balance.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**pending\_balance** `number` *required*

The amount of the balance that is pending.

Example: `6.9`

**reserve\_balance** `number` *required*

The amount of the balance that is reserved.

Example: `6.9`

**id** `string` *required*

The unique identifier for the ledger account.

Example: `ldgr_xxxxxxxxxxxxx`

**ledger\_type** `LedgerTypes` *required*

The type of ledger account.

Available options: `primary`, `pool`

**owner** `User or Company | null` *required*

The owner of the ledger account.

**User**

**id** `string` *required*

The unique identifier for the user.

Example: `user_xxxxxxxxxxxxx`

**name** `string | null` *required*

The user's display name shown on their public profile.

Example: `Marcus Webb`

**typename** `string` *required*

The typename of this object

**username** `string` *required*

The user's unique username shown on their public profile.

Example: `marcuswebb`

**Company**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**route** `string` *required*

URL slug for the account's store page. For example, `shine-time-auto-detailing` in whop.com/shine-time-auto-detailing.

Example: `shine-time-auto-detailing`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**typename** `string` *required*

The typename of this object

**payments\_approval\_status** `PaymentsApprovalStatuses | null` *required*

The status of payments approval for the ledger account.

Available options: `pending`, `approved`, `monitoring`, `rejected`

**payout\_account\_details** `object | null` *required*

The payout account associated with the LedgerAccount, if any.

**child attributes**

**address** `object | null` *required*

The physical address associated with this payout account

**child attributes**

**city** `string | null` *required*

The city of the address.

**country** `string | null` *required*

The country of the address.

**line1** `string | null` *required*

The line 1 of the address.

**line2** `string | null` *required*

The line 2 of the address.

**postal\_code** `string | null` *required*

The postal code of the address.

**state** `string | null` *required*

The state of the address.

**business\_name** `string | null` *required*

The company's legal name

**business\_representative** `object | null` *required*

The business representative for this payout account

**child attributes**

**date\_of\_birth** `string | null` *required*

The date of birth of the business representative in ISO 8601 format
(YYYY-MM-DD).

**first\_name** `string | null` *required*

The first name of the business representative.

**last\_name** `string | null` *required*

The last name of the business representative.

**middle\_name** `string | null` *required*

The middle name of the business representative.

**email** `string | null` *required*

The email address of the representative

**id** `string` *required*

The unique identifier for the payout account.

Example: `poact_xxxxxxxxxxxx`

**latest\_verification** `object | null` *required*

The latest verification for the connected account.

**child attributes**

**id** `string` *required*

The numeric id of the verification record.

Example: `verf_xxxxxxxxxxxxx`

**last\_error\_code** `VerificationErrorCodes | null` *required*

The most recent error code returned during verification. Null if no error has occurred.

Available options: `abandoned`, `consent_declined`, `country_not_supported`, `device_not_supported`, `document_expired`, `document_type_not_supported`, `document_unverified_other`, `email_unverified_other`, `email_verification_declined`, `id_number_insufficient_document_data`, `id_number_mismatch`, `id_number_unverified_other`, `phone_unverified_other`, `phone_verification_declined`, `selfie_document_missing_photo`, `selfie_face_mismatch`, `selfie_manipulated`, `selfie_unverified_other`, `under_supported_age`

**last\_error\_reason** `string | null` *required*

A human-readable explanation of the most recent verification error. Null if no error has occurred.

Example: `Document image was too blurry to read.`

**status** `VerificationStatuses` *required*

The current status of this verification session.

Available options: `requires_input`, `processing`, `verified`, `canceled`, `created`, `started`, `submitted`, `approved`, `declined`, `resubmission_requested`, `expired`, `abandoned`, `review`, `action_required`, `manual_review`

**phone** `string | null` *required*

The business representative's phone

**status** `PayoutAccountCalculatedStatuses | null` *required*

The granular calculated status of the payout account reflecting its current KYC and payout readiness state.

Available options: `connected`, `disabled`, `action_required`, `pending_verification`, `verification_failed`, `manual_review`, `denied`, `not_started`, `blocked_by_parent`

**payout\_quote\_required** `boolean` *required*

Whether legacy payout clients should confirm a provider-backed quote for this
account. REST POST /payouts on API version 2026-11-03 or later always requires
quote\_token from POST /payouts/quotes, regardless of this flag.

**settlement\_time\_at** `string\<date-time\> | null` *required*

The settlement batch most recently posted to this account's available balance, at midnight UTC. Every payment settling in that batch carries the same `settlement_time_at`.

Example: `2023-12-01T05:00:00.401Z`

**transfer\_fee** `number | null` *required*

The fee for transfers, if applicable.

Example: `6.9`

**treasury\_balance** `object | null` *required*

The balance cache associated with the account by currency.

**child attributes**

**balance** `number` *required*

The amount of the balance.

Example: `6.9`

**balance\_usd** `number` *required*

The balance converted to USD.

Example: `6.9`

**currency** `Currencies` *required*

The currency of the balance.

Available options: `usd`, `sgd`, `inr`, `aud`, `brl`, `cad`, `dkk`, `eur`, `nok`, `gbp`, `sek`, `chf`, `hkd`, `huf`, `jpy`, `mxn`, `myr`, `pln`, `czk`, `nzd`, `aed`, `eth`, `ape`, `cop`, `ron`, `thb`, `bgn`, `idr`, `dop`, `php`, `try`, `krw`, `twd`, `vnd`, `pkr`, `clp`, `uyu`, `ars`, `zar`, `dzd`, `tnd`, `mad`, `kes`, `kwd`, `jod`, `all`, `xcd`, `amd`, `bsd`, `bhd`, `bob`, `bam`, `khr`, `crc`, `xof`, `egp`, `etb`, `gmd`, `ghs`, `gtq`, `gyd`, `ils`, `jmd`, `mop`, `mga`, `mur`, `mdl`, `mnt`, `nad`, `ngn`, `mkd`, `omr`, `pyg`, `pen`, `qar`, `rwf`, `sar`, `rsd`, `lkr`, `tzs`, `ttd`, `uzs`, `rub`, `btc`, `cny`, `usdt`, `kzt`, `awg`, `whop_usd`, `xau`, `hnl`

**pending\_balance** `number` *required*

The amount of the balance that is pending.

Example: `6.9`

**reserve\_balance** `number` *required*

The amount of the balance that is reserved.

Example: `6.9`

**total\_withdrawable\_balance** `number` *required*

The amount of the balance that is withdrawable.

Example: `6.9`


## Related topics

- [Retrieve ledger account](https://docs.whop.com/api-reference/legacy/ledger-accounts/retrieve-ledger-account.md)
- [Ledger account funds available](https://docs.whop.com/api-reference/legacy/ledger-accounts/ledger-account-funds-available.md)
- [Whop API Core Concepts](https://docs.whop.com/developer/concepts.md)
