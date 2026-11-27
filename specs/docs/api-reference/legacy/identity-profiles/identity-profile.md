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

# Identity Profile

> A consolidated identity or business profile synced from verification provider data.

```json Example theme={null}
{
	"business_address": {
		"city": "<string>",
		"country": "<string>",
		"line1": "<string>",
		"line2": "<string>",
		"postal_code": "<string>",
		"state": "<string>"
	},
	"business_name": "<string>",
	"business_structure": "<string>",
	"country": "<string>",
	"created_at": "2023-12-01T05:00:00.401Z",
	"date_of_birth": "<string>",
	"email": "<string>",
	"first_name": "<string>",
	"id": "idpf_xxxxxxxxxxxxx",
	"last_name": "<string>",
	"linked_companies": [
		{
			"id": "biz_xxxxxxxxxxxxxx",
			"title": "Shine Time Auto Detailing"
		}
	],
	"payout_status": "connected",
	"payouts_enabled": true,
	"personal_address": {
		"city": "<string>",
		"country": "<string>",
		"line1": "<string>",
		"line2": "<string>",
		"postal_code": "<string>",
		"state": "<string>"
	},
	"phone": "<string>",
	"profile_type": "<string>",
	"status": "not_started",
	"updated_at": "2023-12-01T05:00:00.401Z",
	"verifications": [
		{
			"created_at": "2023-12-01T05:00:00.401Z",
			"id": "verf_xxxxxxxxxxxxx",
			"last_error_code": "abandoned",
			"last_error_reason": "Document image was too blurry to read.",
			"session_url": "https://verify.stripe.com/session/abc123",
			"status": "requires_input"
		}
	]
}
```

**business\_address** `object | null` *required*

Registered business address reported by the identity provider. Present on `business` profiles.

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

Business entity name. Present on `business` profiles.

**business\_structure** `string | null` *required*

Reported legal structure of a business profile (e.g. `corp`, `llc`).
Provider-specific values; present on `business` profiles.

**country** `string | null` *required*

ISO 3166-1 alpha-2 country code reported by the identity provider, such as
`US` or `GB`. For individuals this is the country of citizenship or residence;
for businesses, the country of incorporation.

**created\_at** `string\<date-time\>` *required*

When the identity profile was first created.

Example: `2023-12-01T05:00:00.401Z`

**date\_of\_birth** `string | null` *required*

ISO date (`YYYY-MM-DD`) reported by the identity provider. Present on
`individual` profiles.

**email** `string | null` *required*

Email address reported by the identity provider. Typically present on
`individual` profiles.

**first\_name** `string | null` *required*

Individual's first name.

**id** `string` *required*

The tag of the identity profile (idpf\_xxx).

Example: `idpf_xxxxxxxxxxxxx`

**last\_name** `string | null` *required*

Individual's last name.

**linked\_companies** `array\<object\>` *required*

The companies this identity profile is currently linked to. Only populated for direct Whop user sessions; always empty when authenticated via API key, app, or OAuth scope (a single identity can be linked to companies the calling platform is not entitled to see).

**child attributes**

**id** `string` *required*

The unique identifier for the company.

Example: `biz_xxxxxxxxxxxxxx`

**title** `string` *required*

The display name of the company shown to customers.

Example: `Shine Time Auto Detailing`

**payout\_status** `PayoutAccountCalculatedStatuses` *required*

Progress of payout-account setup for this profile, independent of holds. `connected` means onboarding is complete; a `connected` status paired with `payouts_enabled: false` indicates an active account restriction rather than incomplete setup.

Available options: `connected`, `disabled`, `action_required`, `pending_verification`, `verification_failed`, `manual_review`, `denied`, `not_started`, `blocked_by_parent`

**payouts\_enabled** `boolean` *required*

Whether this profile can receive payouts right now. True only when payout
onboarding is complete and no payout holds are active on the linked account.
Treat this as the single source of truth for payout readiness.

**personal\_address** `object | null` *required*

Residential address reported by the identity provider. Present on `individual` profiles.

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

**phone** `string | null` *required*

Phone number reported by the identity provider. Typically present on
`individual` profiles.

**profile\_type** `string` *required*

Whether this is an 'individual' or 'business' profile.

**status** `IdentityProfileStatuses` *required*

Derived verification status across all linked verifications. Returns `action_required` whenever the profile has an open request for information (whether a verification, payout, or audit RFI) — i.e. the merchant must submit something before it is in good standing.

Available options: `not_started`, `pending`, `manual_review`, `approved`, `rejected`, `action_required`

**updated\_at** `string\<date-time\>` *required*

When the identity profile was last synced from a verification.

Example: `2023-12-01T05:00:00.401Z`

**verifications** `array\<object\>` *required*

All verification attempts attached to this identity profile, ordered most-recent first.

**child attributes**

**created\_at** `string\<date-time\>` *required*

When the verification record was created.

Example: `2023-12-01T05:00:00.401Z`

**id** `string` *required*

The numeric id of the verification record.

Example: `verf_xxxxxxxxxxxxx`

**last\_error\_code** `VerificationErrorCodes | null` *required*

The most recent error code returned during verification. Null if no error has occurred.

Available options: `abandoned`, `consent_declined`, `country_not_supported`, `device_not_supported`, `document_expired`, `document_type_not_supported`, `document_unverified_other`, `email_unverified_other`, `email_verification_declined`, `id_number_insufficient_document_data`, `id_number_mismatch`, `id_number_unverified_other`, `phone_unverified_other`, `phone_verification_declined`, `selfie_document_missing_photo`, `selfie_face_mismatch`, `selfie_manipulated`, `selfie_unverified_other`, `under_supported_age`

**last\_error\_reason** `string | null` *required*

A human-readable explanation of the most recent verification error. Null if no error has occurred.

Example: `Document image was too blurry to read.`

**session\_url** `string | null` *required*

A URL the user can visit to complete the verification process. Null if the session does not require user interaction.

Example: `https://verify.stripe.com/session/abc123`

**status** `VerificationStatuses` *required*

The current status of this verification session.

Available options: `requires_input`, `processing`, `verified`, `canceled`, `created`, `started`, `submitted`, `approved`, `declined`, `resubmission_requested`, `expired`, `abandoned`, `review`, `action_required`, `manual_review`


## Related topics

- [Identity profile updated](https://docs.whop.com/api-reference/legacy/identity-profiles/identity-profile-updated.md)
- [Unlink an identity profile](https://docs.whop.com/api-reference/legacy/identity-profiles/unlink-an-identity-profile.md)
- [Retrieve identity profile](https://docs.whop.com/api-reference/legacy/identity-profiles/retrieve-identity-profile.md)
