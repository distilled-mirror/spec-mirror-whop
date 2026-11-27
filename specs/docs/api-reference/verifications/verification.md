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

# Verifications API

> A Verification represents a legal identity for a person or business.

Accounts and users complete verification when Whop needs to confirm who they are before enabling payouts or compliance-sensitive workflows.

Use the Verifications API to start or resume a hosted verification session, check review status, and submit requested details or documents. If `requested_information` contains items, submit answers with [Update Verification](https://docs.whop.com/api-reference/verifications/update-verification.md).

> [!NOTE]
>
> Replaces the Legacy [Verifications](https://docs.whop.com/api-reference/verifications/verification)
> resource. Existing Legacy integrations keep working; see [API
> versions](https://docs.whop.com/developer/api/versioning.md) for the stability contract.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Verifications](https://docs.whop.com/api-reference/verifications/list-verifications.md) | GET `/verifications` |
| [Retrieve Verification](https://docs.whop.com/api-reference/verifications/retrieve-verification.md) | GET `/verifications/{id}` |
| [Create Verification](https://docs.whop.com/api-reference/verifications/create-verification.md) | POST `/verifications` |
| [Generate Verification Document URL](https://docs.whop.com/api-reference/verifications/generate-verification-document-url.md) | POST `/verifications/{id}/generate_document_url` |
| [Update Verification](https://docs.whop.com/api-reference/verifications/update-verification.md) | PATCH `/verifications/{id}` |

## Attributes

**id** `string`

Verification profile ID, prefixed `idpf_`.

**account\_ids** `string[]` *required*

Accounts this verification was read for, each prefixed `biz_`: the account you
listed by, the account or subaccount it belongs to with `include_subaccounts`,
or every account you can read that it belongs to when retrieved by ID. Usually
one. Empty for a personal verification.

**address** `object | null`

Address on the verification profile. `null` when no address is set.

> [accordion] Properties
>
> **city** `string | null`
>
> **country** `string | null`
>
> Two-letter ISO 3166-1 country code, for example `US`, `DE`, or `GB`.
>
> **line1** `string`
>
> First line of the street address.
>
> **line2** `string | null`
>
> Second line of the street address.
>
> **postal\_code** `string | null`
>
> Postal or ZIP code.
>
> **state** `string | null`
>
> State, province, or region code, for example `CA`.

**business\_name** `string | null`

Legal business name.

**business\_structure** `string | null`

Legal entity structure of the business, such as `private_corporation` or
`sole_proprietorship`. Supported values vary by country of incorporation — see
[Business structures](https://docs.whop.com/developer/verification/business-structures.md).

**country** `string | null`

Two-letter ISO 3166-1 country code, for example `US`, `DE`, or `GB`.

**created\_at** `string`

When the verification profile was created, as an ISO 8601 timestamp.

**date\_of\_birth** `string | null`

Formatted as `YYYY-MM-DD`.

**email** `string | null`

Email address on the verification profile.

**first\_name** `string | null`

**identity\_document** `object | null`

Identity document on file for the person. Individual verifications only. `null` until the person sends a document.

> [accordion] Properties
>
> **country** `string | null` *required*
>
> Two-letter ISO 3166-1 code of the country that issued the document.
>
> **status** `string` *required*
>
> Review result of the document. `pending` until the review ends.
>
> Available options: `pending`, `approved`, `rejected`
>
> **type** `string` *required*
>
> Kind of identity document.
>
> Available options: `id_card`, `drivers`, `residence_permit`, `passport`

**incorporation\_date** `string | null`

Date the business was incorporated, formatted as `YYYY-MM-DD`, as on file for
the business verification. Business verifications only; `null` until it is
sent.

**kind** `string`

Available options: `individual`, `business`

**last\_name** `string | null`

**owners** `object[]` *required*

Owners of the business and the progress of each one's own verification. Empty for individual verifications.

> [accordion] Properties
>
> **id** `string` *required*
>
> Owner ID, prefixed `own_`. Send it in Update Verification's `owners` to change
> or remove the owner, including one added in the hosted session.
>
> **date\_of\_birth** `string | null` *required*
>
> The owner's date of birth on file, formatted as `YYYY-MM-DD`.
>
> **first\_name** `string | null` *required*
>
> **last\_name** `string | null` *required*
>
> **ownership\_percentage** `number | null` *required*
>
> Share of the business the person owns, from 0 to 100.
>
> **rejected\_checks** `string[]` *required*
>
> What the provider rejected when the owner's verification was sent back: `details` (their name, date of birth, or other details don't match their ID), `identity_document`, or `selfie`. Fix only these with Update Verification. Empty otherwise.
>
> Available options: `details`, `identity_document`, `selfie`
>
> **required\_documents** `object[]` *required*
>
> The owner's documents and their progress. Empty for an owner added with `verification_id`.
>
> > [accordion] Properties
> >
> > **document** `string`
> >
> > Document slot key, such as `id_card_front`, `id_card_back`, or `selfie`.
> >
> > **rejection\_reason** `string | null`
> >
> > Why the previous submission was rejected, when the provider requested new
> > documents or declined the verification.
> >
> > **status** `string`
> >
> > `pending_upload` until the document has been relayed for review; `submitted` afterwards.
> >
> > Available options: `pending_upload`, `submitted`
>
> **roles** `string[]` *required*
>
> What the person is to the business.
>
> Available options: `ubo`, `director`, `representative`, `shareholder`
>
> **session\_url** `string | null` *required*
>
> Hosted verification session URL for the owner to complete their own identity
> checks. Expires 7 days after creation. `null` unless the business review waits
> on this owner to start or finish a hosted session.
>
> **status** `string` *required*
>
> Where the owner's own verification stands, in the same states as `status`. `action_required` when their documents need sending again.
>
> Available options: `not_started`, `pending`, `processing`, `manual_review`, `approved`, `rejected`, `action_required`

**phone** `string | null`

Phone number on the verification profile.

**place\_of\_incorporation** `string | null`

State or region where the business is incorporated, as on file for the
business verification. Business verifications only; `null` until it is sent.

**registration\_number** `string | null`

Number the business was registered under in its place of incorporation, such
as a state file number in the US or a Company Number in the UK. Business
verifications only; as on file for the business verification, or `null` until
it is sent.

**requested\_information** `object[]`

What Whop still needs before review can continue, with one requirement per entry. Answer with Update Verification. Nothing from the response is echoed back. Keys that don't apply are omitted. For a business verification created with `documents` or `owners`, this includes the business details and documents still missing before it can be sent for review. Each owner's documents are listed under that owner.

> [accordion] Properties
>
> **id** `string` *required*
>
> Requested information item ID, prefixed `inrqi_`.
>
> **action\_url** `string`
>
> URL for a related action, such as completing liveness verification or viewing
> a payment. Absent when no action is available.
>
> **alternative\_document\_label** `string`
>
> A document that answers this item in place of `value`, such as an IRS EIN
> letter when the EIN could not be matched. Send its pages in `files`, with or
> without a corrected `value`. Absent when only `value` is accepted.
>
> **details\_label** `string`
>
> Follow-up prompt shown with this requirement.
>
> **details\_required** `boolean`
>
> Whether the follow-up response is required when visible.
>
> **details\_visible\_for** `string[]`
>
> Selected option values that make the follow-up prompt visible.
>
> **errors** `object[]`
>
> Present after a rejected submission.
>
> > [accordion] Properties
> >
> > **code** `string`
> >
> > Stable error code.
> >
> > **reason** `string`
> >
> > Why it was rejected.
>
> **label** `string` *required*
>
> Instruction to show the user. Carries the reviewer's note verbatim when there
> is one.
>
> **optional** `boolean`
>
> `true` when the item can be skipped.
>
> **options** `string[]`
>
> The values `value` may take on a `select` item. On an `id_document` item these
> are the ID types accepted, and the chosen one decides which `documents` slots
> to send. Absent when the item has no choice to make.
>
> **requirement** `string` *required*
>
> What is needed: a document name such as `bank_statement`, or a field key such
> as `ssn` or `identity_document`. Handle unrecognized values by `type`.
>
> **response\_type** `string`
>
> Optional native input format for a text response.
>
> Available options: `yes_no`, `yes_no_na`, `date`, `url`, `number`, `tel`
>
> **selection\_mode** `string`
>
> Whether a question with `options` accepts one value or multiple values.
>
> Available options: `single`, `multiple`
>
> **supporting\_documents** `object[]`
>
> Documents supplied with the requirement for context.
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
> > The original file's MIME type, for example `application/pdf`. It is kept when
> > moderation removes the file.
> >
> > **created\_at** `string` *required*
> >
> > When the file was created, as an ISO 8601 timestamp.
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
> **supporting\_files\_explanation\_allowed** `boolean`
>
> Whether a written explanation may replace required supporting files.
>
> **supporting\_files\_required** `boolean`
>
> Whether this requirement also needs supporting files.
>
> **supporting\_files\_visible\_for** `string[]`
>
> Selected option values that make the supporting-file input visible.
>
> **type** `string` *required*
>
> What to send as the answer, so you never have to infer it: `files` (a document, as a list of its pages), `id_document` (send `documents` with the slot keys for the ID you are uploading), `text`, `date`, `phone` or `select` (send `value`), `text_with_files` (send `value` and optional `files`), `address` (send `address`), or `liveness` (open `action_url`, then send `value` as `true` after completion).

**required\_documents** `object[]`

Documents for a document-upload verification and their progress. Present only on individual verifications created by sending `documents`. `pending_upload` documents were not accepted yet — send the full set again with another Create Verification call.

> [accordion] Properties
>
> **document** `string`
>
> Document slot key, such as `id_card_front`, `id_card_back`, or `selfie`.
>
> **rejection\_reason** `string | null`
>
> Why the previous submission was rejected, when the provider requested new
> documents or declined the verification.
>
> **status** `string`
>
> `pending_upload` until the document has been relayed for review; `submitted` afterwards.
>
> Available options: `pending_upload`, `submitted`

**selfie** `object | null`

Selfie check of the person. Individual verifications only. `null` when the verification has no selfie check.

> [accordion] Properties
>
> **capture** `string` *required*
>
> How the selfie is taken. `liveness` is a live camera check. `upload` is a photo sent as a file.
>
> Available options: `upload`, `liveness`
>
> **status** `string` *required*
>
> Review result of the selfie check. `pending` until the review ends.
>
> Available options: `pending`, `approved`, `rejected`

**session\_url** `string | null`

Hosted verification session URL for the user to complete identity checks.
Expires 7 days after creation.

**status** `string`

Current verification state. `not_started` before any session exists; `pending` while a session needs the user's input; `processing` while the provider's automated checks run on a fresh submission; `action_required` when `requested_information` needs answers; `manual_review` while information already sent is under review — an audit answer, or a document the payout provider holds — nothing to submit, usually done within 3 business days; `approved` on success; `rejected` on failure. Call Create Verification again to start a new session.

Available options: `not_started`, `pending`, `processing`, `manual_review`, `approved`, `rejected`, `action_required`

**updated\_at** `string`

When the verification profile was last updated, as an ISO 8601 timestamp.

```json Verification theme={null}
{
	"id": "idpf_xxxxxxxxxxxx",
	"account_ids": ["biz_xxxxxxxxxxxxxx"],
	"address": {
		"city": "New York",
		"country": "US",
		"line1": "123 Spring Street",
		"line2": "Apt 4B",
		"postal_code": "10012",
		"state": "NY"
	},
	"owners": [
		{
			"id": "idpf_xxxxxxxxxxxx",
			"first_name": "Marcus",
			"last_name": "Webb",
			"date_of_birth": "1985-06-01",
			"ownership_percentage": 60,
			"required_documents": [
				{
					"document": "passport_front",
					"rejection_reason": null,
					"status": "submitted"
				},
				{
					"document": "selfie",
					"rejection_reason": null,
					"status": "submitted"
				}
			],
			"roles": ["ubo", "director"],
			"status": "processing",
			"session_url": null,
			"rejected_checks": []
		}
	],
	"business_name": null,
	"business_structure": null,
	"country": "US",
	"created_at": "2026-06-01T12:00:00Z",
	"date_of_birth": "1990-01-01",
	"email": "marcus@shinetime.example",
	"first_name": "Marcus",
	"kind": "individual",
	"last_name": "Webb",
	"phone": "+15551234567",
	"registration_number": null,
	"requested_information": [
		{
			"id": "inrqi_xxxxxxxxxxx",
			"label": "Identity document",
			"options": ["PASSPORT", "ID_CARD", "DRIVERS", "RESIDENCE_PERMIT"],
			"requirement": "identity_document",
			"type": "id_document"
		},
		{
			"id": "inrqi_yyyyyyyyyyy",
			"label": "Bank Statement or a check for account ending in 4242",
			"requirement": "bank_statement",
			"type": "files"
		}
	],
	"required_documents": [
		{
			"document": "id_card_front",
			"rejection_reason": null,
			"status": "submitted"
		},
		{
			"document": "id_card_back",
			"rejection_reason": null,
			"status": "submitted"
		},
		{
			"document": "selfie",
			"rejection_reason": null,
			"status": "submitted"
		}
	],
	"session_url": "https://verify.whop.com/session/idpf_xxxxxxxxxxxx",
	"status": "action_required",
	"updated_at": "2026-06-02T12:00:00Z"
}
```


## Related topics

- [Verifications](https://docs.whop.com/elements/latest/verifications/overview.md)
- [Verification](https://docs.whop.com/api-reference/legacy/verifications/verification.md)
- [List verifications](https://docs.whop.com/api-reference/legacy/verifications/list-verifications.md)
