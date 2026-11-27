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

# Verification

> An identity verification session used to confirm a person or entity's identity for payout account eligibility.

> [!NOTE]
>
> This resource has a successor in the [Whop
> API](https://docs.whop.com/api-reference/legacy/verifications/verification.md), the versioned API that
> new integrations should build on. This page stays fully supported.

A Verification guides a person or business through the identity checks required for an account to receive payouts. Individuals complete Know Your Customer (KYC) checks, and businesses complete Know Your Business (KYB) checks. For example, create a Verification before a user's first payout. You can prefill their name and address so they can complete the hosted form without a custom verification UI. Later, use the same Verification to retrieve their verified identity data and respond to [RFIs](https://docs.whop.com/developer/verification/rfis.md) from downstream payout providers.

Create a Verification when an account needs to be verified before transacting — typically before their first [payout](https://docs.whop.com/api-reference/payouts/payout). Before creating, make sure you have an [account](https://docs.whop.com/api-reference/accounts/account) with a ledger account (created automatically when you create the account). Don't create multiple Verifications of the same `kind` on the same account — each account supports at most one active individual (KYC) and one business (KYB) profile. The Verification transitions through multiple statuses as the user completes the process: `pending` while they're going through KYC, `approved` once identity is confirmed, `rejected` if the provider declines, and `action_required` when a payout provider needs additional information like a bank statement or tax ID.

Successful Verifications result in a fully verified identity with confirmed name, date of birth, country, and address — unlocking the account for [payouts](https://docs.whop.com/api-reference/legacy/payout-accounts/payout-account.md). If you create a Verification with pre-fill fields (`first_name`, `address`, etc.), it automatically seeds both the verification and the hosted KYC form so the user doesn't re-enter information you already have. We recommend listening for the `identity_profile.approved` and `identity_profile.rejected` [webhooks](https://docs.whop.com/api-reference/legacy/webhooks/webhook.md) rather than polling, to react to status changes in real time. By using Verifications, you can onboard users for payouts through a single API integration, even as the underlying KYC providers and compliance requirements change over time.

## Endpoints

| Method | Path | Description |
| - | - | - |
| `GET` | `/verifications?account_id={biz_ tag}` | [List verifications](https://docs.whop.com/developer/verification/retrieve.md) for an account |
| `GET` | `/verifications/{verification_id}` | [Retrieve a single verification](https://docs.whop.com/developer/verification/retrieve.md) by its `idpf_` tag |
| `POST` | `/verifications` | [Create a verification](https://docs.whop.com/developer/verification/overview.md) and start a KYC/KYB session |
| `PATCH` | `/verifications/{verification_id}` | [Update identity fields or respond to RFIs](https://docs.whop.com/developer/verification/manage.md) |
| `DELETE` | `/verifications/{verification_id}` | [Soft-delete](https://docs.whop.com/developer/verification/manage.md) and unlink from the account |

## Statuses

| Status | Description | Next step |
| - | - | - |
| `not_started` | Verification exists but no KYC session has been started | Call `POST /verifications` to start |
| `pending` | User needs to complete KYC at the `session_url` | Redirect the user to `session_url` |
| `approved` | KYC passed — the account can transact | No action needed |
| `rejected` | KYC was declined by the provider | Call `POST /verifications` with `restart: true` |
| `action_required` | A payout provider needs additional information | Check `rfis` array and [respond](https://docs.whop.com/developer/verification/rfis.md) |

```json Example theme={null}
{
	"id": "verf_xxxxxxxxxxxxx",
	"last_error_code": "abandoned",
	"last_error_reason": "Document image was too blurry to read.",
	"status": "requires_input"
}
```

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


## Related topics

- [Verifications](https://docs.whop.com/elements/latest/verifications/overview.md)
- [Verifications API](https://docs.whop.com/api-reference/verifications/verification.md)
- [Create Verification](https://docs.whop.com/api-reference/verifications/create-verification.md)
