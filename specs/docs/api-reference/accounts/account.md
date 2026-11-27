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

# Accounts API

> An Account represents a person or business on Whop that can have its own profile, wallet, and account-scoped settings.

Use accounts for customers, creators, merchants, sellers, or connected businesses your integration supports.

Use the Accounts API to create accounts, list accounts visible to your credentials, retrieve or update an account, suspend or delete a subaccount managed by your platform, and retrieve the account associated with the current API key.

The `business_type`, `industry_group`, and `industry_type` fields classify accounts. See the [business types and industries glossary](#business-types-and-industries) for every valid value.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Accounts](https://docs.whop.com/api-reference/accounts/list-accounts.md) | GET `/accounts` |
| [Retrieve Account](https://docs.whop.com/api-reference/accounts/retrieve-account.md) | GET `/accounts/{id}` |
| [Retrieve Account Affiliate Program](https://docs.whop.com/api-reference/accounts/retrieve-account-affiliate-program.md) | GET `/accounts/{account_id}/affiliate_program` |
| [Retrieve Account Customer Tracking](https://docs.whop.com/api-reference/accounts/retrieve-account-customer-tracking.md) | GET `/accounts/{account_id}/customer_tracking` |
| [Retrieve Account Fees](https://docs.whop.com/api-reference/accounts/retrieve-account-fees.md) | GET `/accounts/{account_id}/fees` |
| [List Financing Providers](https://docs.whop.com/api-reference/accounts/list-financing-providers.md) | GET `/accounts/{account_id}/financing_providers` |
| [Retrieve Account Preferences](https://docs.whop.com/api-reference/accounts/retrieve-account-preferences.md) | GET `/accounts/{account_id}/preferences` |
| [List Account Reserves](https://docs.whop.com/api-reference/accounts/list-account-reserves.md) | GET `/accounts/{account_id}/reserves` |
| [Get Appeal Eligibility](https://docs.whop.com/api-reference/accounts/get-appeal-eligibility.md) | GET `/suspension_appeals/context` |
| [Create Account](https://docs.whop.com/api-reference/accounts/create-account.md) | POST `/accounts` |
| [Form Company](https://docs.whop.com/api-reference/accounts/form-company.md) | POST `/accounts/{id}/form_company` |
| [Retry Failed Ads Payments](https://docs.whop.com/api-reference/accounts/retry-failed-ads-payments.md) | POST `/accounts/{id}/retry_ads_payment` |
| [Suspend a Subaccount](https://docs.whop.com/api-reference/accounts/suspend-a-subaccount.md) | POST `/accounts/{id}/suspend` |
| [Transfer Account Ownership](https://docs.whop.com/api-reference/accounts/transfer-account-ownership.md) | POST `/accounts/{id}/transfer_ownership` |
| [Submit a Suspension Appeal](https://docs.whop.com/api-reference/accounts/submit-a-suspension-appeal.md) | POST `/suspension_appeals` |
| [Update Account](https://docs.whop.com/api-reference/accounts/update-account.md) | PATCH `/accounts/{id}` |
| [Update Account Fees](https://docs.whop.com/api-reference/accounts/update-account-fees.md) | PATCH `/accounts/{account_id}/fees` |
| [Update Account Preferences](https://docs.whop.com/api-reference/accounts/update-account-preferences.md) | PATCH `/accounts/{account_id}/preferences` |
| [Delete a Subaccount](https://docs.whop.com/api-reference/accounts/delete-a-subaccount.md) | DELETE `/accounts/{id}` |

## Fee markups

Fee markups let a platform add charges or subsidize payout fees for its subaccounts. Manage them through the account fees document: set defaults on the platform account or override individual fees on a subaccount. See [Fee markups & subsidies](https://docs.whop.com/developer/platforms/fee-markups.md) for examples, limits, and funding requirements.

### Fee endpoints

| Action | Endpoint | SDK Method |
| - | - | - |
| [Retrieve account fees](https://docs.whop.com/api-reference/accounts/retrieve-account-fees.md) | `GET /api/v1/accounts/{account_id}/fees` | `client.accounts.fees.retrieve(accountId)` |
| [Update account fees](https://docs.whop.com/api-reference/accounts/update-account-fees.md) | `PATCH /api/v1/accounts/{account_id}/fees` | `client.accounts.fees.update(accountId, changes)` |

Use the platform account's API key with `company:update_child_fees` permission to configure markups. Each returned markup includes `adjustable` and `unadjustable_reason` so your integration can determine whether editing is available.

### Read fees

```typescript theme={null}
const fees = await client.accounts.fees.retrieve("biz_connected_account");
```

The `markups` section contains the subaccount's effective rates. The `child_markups` section on a platform account contains its defaults for subaccounts.

A markup's `source` is `custom` when configured at that level, or `default` when it falls through to an inherited rate or zero. Its `default` field shows the rate that would apply if the custom markup were cleared. Returned fixed amounts use the Money object; updates accept `fixed` as a number in US dollars.

### Set platform defaults

Update `child_markups` on the platform account. This example sets a 2% payment markup with no fixed charge:

```typescript theme={null}
await client.accounts.fees.update("biz_platform_id", {
	child_markups: { payments: { percentage: 2, fixed: 0 } },
});
```

`percentage: 2` means 2%. Defaults apply to subaccounts without an override for that fee. Payment markups apply when no checkout application fee is set.

### Set a Subaccount override

Update `markups` on the subaccount. This example replaces its inherited payment markup with 1% plus \$0.25:

```typescript theme={null}
await client.accounts.fees.update("biz_connected_account", {
	markups: { payments: { percentage: 1, fixed: 0.25 } },
});
```

Only the keys you send change. For other transaction types, use `transfers`, `crypto_swaps`, `deposits` keyed by rail, or `payouts` keyed by payout method. See the [update endpoint](https://docs.whop.com/api-reference/accounts/update-account-fees.md) for the supported keys and request schema.

### Clear a markup

Send `null` for the markup to clear it. Clearing a subaccount's override restores its platform default, or zero if no default is configured:

```typescript theme={null}
await client.accounts.fees.update("biz_connected_account", {
	markups: { payments: null },
});
```

To clear a platform default, send `child_markups: { payments: null }` to the platform account. An explicit `{ percentage: 0, fixed: 0 }` remains a configured rate; use `null` to restore inheritance.

## Legal documents

An account publishes its legal documents as PDF files: `terms_of_service`, `privacy_policy`, `return_policy`, `cancellation_policy`, `shipping_policy`, and `eula`. Each reads back as a [File](https://docs.whop.com/api-reference/files/file) and is set through [Update Account](https://docs.whop.com/api-reference/accounts/update-account.md) with the `id` of a file you [uploaded](https://docs.whop.com/developer/guides/upload-files.md).

New disputes default their policy evidence from these documents. The cancellation policy slot takes `cancellation_policy`, then `terms_of_service`, then `return_policy`. The refund policy slot takes `return_policy`, then `terms_of_service`. Either slot falls back to Whop's platform policy when none is set, and `shipping_policy` rides along on physical-goods disputes. Set them once here, and use [Upload Dispute Evidence](https://docs.whop.com/api-reference/disputes/upload-dispute-evidence.md) only when one dispute needs a different version of a policy.

```typescript TypeScript theme={null}
import { readFile } from "node:fs/promises";

const file = await client.files.create({ filename: "terms-of-service.pdf" });
await fetch(file.upload_url!, {
	method: "PUT",
	headers: file.upload_headers as Record<string, string>,
	body: await readFile("./terms-of-service.pdf"),
});

await client.accounts.update({
	id: "biz_xxxxxxxxxxxxx",
	terms_of_service: { id: file.id },
});
```

```python Python theme={null}
import requests

file = client.files.create(filename="terms-of-service.pdf")
with open("./terms-of-service.pdf", "rb") as contents:
    requests.put(file.upload_url, headers=file.upload_headers or {}, data=contents)

client.accounts.update(
    "biz_xxxxxxxxxxxxx",
    terms_of_service={"id": file.id},
)
```

Send `null` for a document to remove it. Only PDF files are accepted.

## Attributes

**id** `string` *required*

Account ID, prefixed `biz_`.

**ads\_spend\_usd** `number | null` *required*

Lifetime charged ad spend for the account, in USD. Computed only on `list` for
callers with `stats:read` on the account, and `null` otherwise.

**balances** `object[]` *required*

Account holdings, each with USD value. Empty when `total_usd` is `null`.

> [accordion] Properties
>
> **balance** `string` *required*
>
> Total amount held in native units, as a decimal string.
>
> **breakdown** `object` *required*
>
> Balance split into available, pending, and reserve amounts, as native-unit decimal strings, with the days the pending amount is expected to settle.
>
> > [accordion] Properties
> >
> > **available** `string` *required*
> >
> > Amount you can spend, send, or withdraw now, in native units, as a decimal
> > string.
> >
> > **finishing\_settlement** `string` *required*
> >
> > The part of `pending` that has already settled and is waiting to move into the
> > account's crypto wallet or Whop Card, where it becomes `available`. The move
> > usually starts within the hour; while it runs, the amount is reported in
> > `in_transit` instead. Money moved only to top up the account's retained
> > minimum balance is not included. In native units, as a decimal string.
> >
> > **in\_transit** `string` *required*
> >
> > Amount moving between the account's own destinations, such as a card top-up.
> > In native units, as a decimal string.
> >
> > **pending** `string` *required*
> >
> > Amount from recent payments still settling, in native units, as a decimal
> > string.
> >
> > **pending\_settlements** `object[]` *required*
> >
> > When the pending amount is expected to settle, one entry per day, earliest first. Money without a scheduled settlement day, including `finishing_settlement`, is left out, so these entries plus `finishing_settlement` can sum to less than `pending`, never more.
> >
> > > [accordion] Properties
> > >
> > > **amount** `string` *required*
> > >
> > > Amount expected that day, in native units, as a decimal string.
> > >
> > > **date** `string` *required*
> > >
> > > The day this money is expected to finish settling, as an ISO 8601 date.
> >
> > **perps** `string` *required*
> >
> > Amount held for perpetual trading, at what the trading account is worth now,
> > in native units, as a decimal string. It is not part of `available`.
> >
> > **reserve** `string` *required*
> >
> > Amount held back, in native units, as a decimal string. Retrieve the account's reserves for why it is held and when it unlocks.
>
> **icon\_url** `string | null` *required*
>
> Holding icon URL.
>
> **name** `string` *required*
>
> The holding's display name
>
> **price\_usd** `number | null` *required*
>
> USD price per unit, or `null` when no exchange rate is available.
>
> **symbol** `string` *required*
>
> Holding display symbol, such as `USDT`, `cbBTC`, or `EUR`.
>
> **value\_usd** `string | null` *required*
>
> Holding USD value, or `null` when no exchange rate is available.

**banner\_image\_url** `string | null` *required*

Account banner image URL.

**business\_address** `object | null` *required*

Account business address used to calculate tax, with `line1`, `line2`, `city`,
`state`, `postal_code`, and `country`. `null` when no address is set.

**business\_name** `string | null` *required*

The account's legal business name used with its tax address.

**business\_type** `string | null` *required*

High-level business category for the account. See the [business types and
industries
glossary](https://docs.whop.com/api-reference/accounts/account#business-types-and-industries-glossary)
for valid values.

**can\_transfer\_pending\_balance\_to\_children** `boolean` *required*

Whether pending funds may be transferred from this platform account to its
subaccounts.

**cancellation\_policy** `object | null` *required*

The account's cancellation policy document, or `null` if they have not published one.

> [accordion] Properties
>
> **id** `string` *required*
>
> The file's ID, prefixed `file_`.
>
> **blurhash** `string | null` *required*
>
> BlurHash placeholder for an image or video. Null when unavailable or removed
> by moderation.
>
> **content\_type** `string | null` *required*
>
> The original file's MIME type, for example `application/pdf`. It is kept when
> moderation removes the file.
>
> **created\_at** `string` *required*
>
> When the file was created, as an ISO 8601 timestamp.
>
> **duration\_seconds** `number | null` *required*
>
> Audio or video duration in seconds. Null when unavailable or removed by
> moderation.
>
> **filename** `string | null` *required*
>
> The original filename, including its extension.
>
> **height** `integer | null` *required*
>
> Image or video height in pixels. Null when unavailable or removed by
> moderation.
>
> **multipart\_chunk\_size** `integer | null`
>
> The byte size each part (except the last) must be. Present only on create, and
> only for multipart uploads.
>
> **multipart\_upload\_id** `string | null`
>
> The ID of the multipart upload, passed back to `complete`. Present only on
> create, and only for multipart uploads.
>
> **multipart\_upload\_urls** `object[] | null`
>
> The presigned URL for each part. Present only on create, and only for multipart uploads.
>
> > [accordion] Properties
> >
> > **part\_number** `integer` *required*
> >
> > The 1-based index of this part within the multipart upload.
> >
> > **url** `string` *required*
> >
> > The presigned URL to PUT this part's bytes to.
>
> **object** `string` *required*
>
> The type of this object, always `file`.
>
> **optimized\_url** `string | null` *required*
>
> URL to the optimized file. `null` when no optimized version is available.
>
> **removed\_by\_moderation** `boolean` *required*
>
> Whether moderation removed the file. When `true`, `url` serves a replacement
> image. `filename`, `size` and `content_type` still describe the original file.
>
> **size** `integer | null` *required*
>
> The file size in bytes. `null` until the upload has finished.
>
> **upload\_headers** `object`
>
> Headers to send with the upload PUT. Present only on create.
>
> **upload\_status** `string` *required*
>
> Where the file is in its upload lifecycle.
>
> Available options: `pending`, `processing`, `ready`, `failed`
>
> **upload\_url** `string | null`
>
> Presigned URL to PUT the file's bytes to. Present only on create, and only for
> single-part uploads.
>
> **url** `string | null` *required*
>
> A URL to download the file. Public files get a permanent CDN URL, and private
> files get a signed URL that expires. `null` until the upload has finished.
> When moderation removes the file, a replacement image URL.
>
> **visibility** `string` *required*
>
> `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
>
> Available options: `public`, `private`
>
> **waveform\_url** `string | null` *required*
>
> Audio waveform URL. Null when unavailable or removed by moderation.
>
> **width** `integer | null` *required*
>
> Image or video width in pixels. Null when unavailable or removed by moderation.

**capabilities** `object | null` *required*

Payment rails enabled for this account, each `active`, `inactive`, or `pending` (onboarding or review in progress). Computed only on `retrieve` and `me` for callers with `company:balance:read` scope; `null` otherwise.

> [accordion] Properties
>
> **accept\_bank\_payments** `string` *required*
>
> Bank payins: debits, transfers, and local bank rails
>
> Available options: `active`, `inactive`, `pending`
>
> **accept\_bnpl\_payments** `string` *required*
>
> Buy-now-pay-later payins; requires approval
>
> Available options: `active`, `inactive`, `pending`
>
> **accept\_card\_payments** `string` *required*
>
> Card payins, including Apple Pay and Google Pay
>
> Available options: `active`, `inactive`, `pending`
>
> **bank\_deposit** `string` *required*
>
> Deposits by bank wire or ACH to the account's virtual bank account
>
> Available options: `active`, `inactive`, `pending`
>
> **card\_deposit** `string` *required*
>
> Balance top-ups by charging a stored payment method
>
> Available options: `active`, `inactive`, `pending`
>
> **card\_issuing** `string` *required*
>
> Issuing Whop cards; requires card application approval
>
> Available options: `active`, `inactive`, `pending`
>
> **crypto\_deposit** `string` *required*
>
> On-chain deposits to the account's crypto wallet
>
> Available options: `active`, `inactive`, `pending`
>
> **crypto\_payout** `string` *required*
>
> On-chain payouts to a crypto wallet
>
> Available options: `active`, `inactive`, `pending`
>
> **instant\_payout** `string` *required*
>
> Instant payouts to an eligible payout destination
>
> Available options: `active`, `inactive`, `pending`
>
> **run\_ads** `string` *required*
>
> Launching ad campaigns through Whop Ads. `inactive` while a requested ads services agreement is awaiting the account's signature, and while the account is suspended from Whop Ads.
>
> Available options: `active`, `inactive`, `pending`
>
> **standard\_payout** `string` *required*
>
> Standard payouts to an external payout destination
>
> Available options: `active`, `inactive`, `pending`
>
> **transfer** `string` *required*
>
> Transfers to other accounts
>
> Available options: `active`, `inactive`, `pending`

**cards** `object | null` *required*

Whop Cards application details for the account. Returned on `list`, `retrieve`, and `me` for callers with `company:balance:read` scope; `null` otherwise, or when the account has no card application or blocking application review.

> [accordion] Properties
>
> **decline\_reason** `string | null` *required*
>
> Reason the card issuer declined the application. `unsupported_state` replaces `unsupported_region` for a verified person or business in the United States. `compliance` covers sanctions, politically exposed person, and adverse media checks. `other` covers reasons that are not in this list. `null` when `status` is not `denied`, or when the issuer did not decline the application.
>
> Available options: `compromised_identity`, `duplicate`, `unsupported_region`, `compliance`, `fraud`, `verification_failed`, `unsupported_state`, `other`
>
> **kind** `string | null` *required*
>
> Whether the card application verifies a business (`business`, KYB) or a person (`individual`, consumer identity). `null` when the application is not yet linked to a verification.
>
> Available options: `individual`, `business`
>
> **status** `string` *required*
>
> Where the card application stands. `approved` means cards can be issued. `needs_verification` means the applicant has not completed identity verification yet. `needs_information` means they did, but the documents were rejected for a fixable reason and must be resubmitted. `pending` and `manual_review` are in flight. `denied`, `locked`, and `canceled` are terminal.
>
> Available options: `approved`, `pending`, `manual_review`, `denied`, `locked`, `canceled`, `needs_verification`, `needs_information`

**collect\_vat\_id** `boolean` *required*

The account's saved preference for offering optional buyer VAT/tax ID entry.
Checkouts where Whop collects and remits tax offer entry automatically,
regardless of this preference. Does not require a VAT ID to purchase.

**company\_formation** `object` *required*

Company formation state for the account, managed through [Form Company](https://docs.whop.com/api-reference/accounts/form-company.md). A `draft` `status` until the formation checkout is paid, then filing progress with downloadable documents and signatures awaiting action. Empty when the formation state is temporarily unavailable.

> [accordion] Properties
>
> **documents** `object[]`
>
> Formation documents available for download, such as the Articles of Organization and the EIN confirmation letter. Present once `status` leaves `draft`.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Document ID, prefixed `file_`.
> >
> > **name** `string` *required*
> >
> > Human-readable document name, such as `Articles of Organization`.
> >
> > **type** `string` *required*
> >
> > Document category: `articles_of_organization`, `operating_agreement`,
> > `ein_letter`, `signed_ss4`, `signed_form8821`, or `mail` for postal
> > correspondence received on the company's behalf.
> >
> > **url** `string` *required*
> >
> > CDN URL for downloading the document.
>
> **ein\_registered** `boolean`
>
> Whether the company's EIN has been issued by the IRS. Present once `status`
> leaves `draft`.
>
> **legal\_name** `string | null`
>
> Registered company name including the entity ending, for example `Shine Time
>           	Auto Detailing, LLC`. Present once `status` leaves `draft`.
>
> **signatures** `object`
>
> IRS forms still awaiting a founder's signature, each with a hosted signing URL. Present once `status` leaves `draft`; empty when nothing needs signing.
>
> > [accordion] Properties
> >
> > **form8821** `object`
> >
> > Signature state for IRS Form 8821, the tax information authorization. Present only while the form still needs the founder's action.
> >
> > > [accordion] Properties
> > >
> > > **expires\_at** `string`
> > >
> > > When the signing URL expires, as an ISO 8601 timestamp. Present only when the
> > > signing URL is included.
> > >
> > > **status** `string` *required*
> > >
> > > `pending` when a signing session is ready for the founder; `unknown` when the signature state could not be determined.
> > >
> > > Available options: `pending`, `unknown`
> > >
> > > **url** `string`
> > >
> > > Hosted signing URL where the founder completes the form. Present while `status` is `pending` and the caller has `incorporation:write`. Omitted from webhooks.
> >
> > **ss4** `object`
> >
> > Signature state for IRS Form SS-4, the EIN application. Present only while the form still needs the founder's action.
> >
> > > [accordion] Properties
> > >
> > > **expires\_at** `string`
> > >
> > > When the signing URL expires, as an ISO 8601 timestamp. Present only when the
> > > signing URL is included.
> > >
> > > **status** `string` *required*
> > >
> > > `pending` when a signing session is ready for the founder; `unknown` when the signature state could not be determined.
> > >
> > > Available options: `pending`, `unknown`
> > >
> > > **url** `string`
> > >
> > > Hosted signing URL where the founder completes the form. Present while `status` is `pending` and the caller has `incorporation:write`. Omitted from webhooks.
>
> **state\_registered** `boolean`
>
> Whether the state formation filing is complete. Present once `status` leaves
> `draft`.
>
> **status** `string`
>
> Available options: `draft`, `processing`, `filed`, `rejected`, `completed`

**country** `string | null` *required*

Country where the account is located.

**created\_at** `string` *required*

When the account was created, as an ISO 8601 timestamp.

**customers\_l30d** `integer | null` *required*

Distinct customers with a paid receipt at the account in the last 30 days,
refreshed every few hours. Computed only on `retrieve` for callers with
`stats:read` on the account; `null` otherwise, and when the count is
unavailable.

**description** `string | null` *required*

Account promotional description.

**direct\_reports\_count** `integer`

How many team members report to the business directly, with no manager above
them. Present on single-account reads.

**dispute\_fighter\_enabled** `boolean` *required*

Whether Whop assembles and files dispute evidence for this account. Enabling
it opts the account into the success fee charged on disputes it wins.

**economic\_intelligence** `boolean` *required*

Whether Economic Intelligence is on for the account. It turns off
automatically when its committed period ends. While it is off, the account
can't activate an experiment or run a recommendation.

**email** `string | null` *required*

Account owner email address.

**eula** `object | null` *required*

The account's end-user license agreement document, or `null` if they have not published one.

> [accordion] Properties
>
> **id** `string` *required*
>
> The file's ID, prefixed `file_`.
>
> **blurhash** `string | null` *required*
>
> BlurHash placeholder for an image or video. Null when unavailable or removed
> by moderation.
>
> **content\_type** `string | null` *required*
>
> The original file's MIME type, for example `application/pdf`. It is kept when
> moderation removes the file.
>
> **created\_at** `string` *required*
>
> When the file was created, as an ISO 8601 timestamp.
>
> **duration\_seconds** `number | null` *required*
>
> Audio or video duration in seconds. Null when unavailable or removed by
> moderation.
>
> **filename** `string | null` *required*
>
> The original filename, including its extension.
>
> **height** `integer | null` *required*
>
> Image or video height in pixels. Null when unavailable or removed by
> moderation.
>
> **multipart\_chunk\_size** `integer | null`
>
> The byte size each part (except the last) must be. Present only on create, and
> only for multipart uploads.
>
> **multipart\_upload\_id** `string | null`
>
> The ID of the multipart upload, passed back to `complete`. Present only on
> create, and only for multipart uploads.
>
> **multipart\_upload\_urls** `object[] | null`
>
> The presigned URL for each part. Present only on create, and only for multipart uploads.
>
> > [accordion] Properties
> >
> > **part\_number** `integer` *required*
> >
> > The 1-based index of this part within the multipart upload.
> >
> > **url** `string` *required*
> >
> > The presigned URL to PUT this part's bytes to.
>
> **object** `string` *required*
>
> The type of this object, always `file`.
>
> **optimized\_url** `string | null` *required*
>
> URL to the optimized file. `null` when no optimized version is available.
>
> **removed\_by\_moderation** `boolean` *required*
>
> Whether moderation removed the file. When `true`, `url` serves a replacement
> image. `filename`, `size` and `content_type` still describe the original file.
>
> **size** `integer | null` *required*
>
> The file size in bytes. `null` until the upload has finished.
>
> **upload\_headers** `object`
>
> Headers to send with the upload PUT. Present only on create.
>
> **upload\_status** `string` *required*
>
> Where the file is in its upload lifecycle.
>
> Available options: `pending`, `processing`, `ready`, `failed`
>
> **upload\_url** `string | null`
>
> Presigned URL to PUT the file's bytes to. Present only on create, and only for
> single-part uploads.
>
> **url** `string | null` *required*
>
> A URL to download the file. Public files get a permanent CDN URL, and private
> files get a signed URL that expires. `null` until the upload has finished.
> When moderation removes the file, a replacement image URL.
>
> **visibility** `string` *required*
>
> `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
>
> Available options: `public`, `private`
>
> **waveform\_url** `string | null` *required*
>
> Audio waveform URL. Null when unavailable or removed by moderation.
>
> **width** `integer | null` *required*
>
> Image or video width in pixels. Null when unavailable or removed by moderation.

**external\_id** `string | null` *required*

The parent platform's own identifier for this subaccount, unique among the
platform's subaccounts. `null` when unset.

**financing** `object | null` *required*

The account's most recent financing application. Computed only on `retrieve` and `me` for callers with `company:balance:read` scope; `null` otherwise, or when the account has never applied for financing.

> [accordion] Properties
>
> **application\_id** `string` *required*
>
> The ID of the account's most recent financing application, starting with
> inrq\_. Retrieve it for requirements and review feedback.
>
> **status** `string` *required*
>
> The most recent financing application's review state: `requires_collection` (waiting on the merchant, including after a request for more information), `awaiting_review`, `approved`, `denied`, or `completed`. Whether financing payment methods are enabled is `capabilities.accept_bnpl_payments`.
>
> Available options: `requires_collection`, `awaiting_review`, `completed`, `approved`, `denied`

**funds\_release\_at** `string | null` *required*

When a suspended account can pay out its balance to its own payout method, as
an ISO 8601 timestamp. A dispute can move it later. Computed on `retrieve`,
`me`, and `suspend`; `null` otherwise, when `status` is not `suspended`, and
when the suspension holds the balance.

**home\_preferences** `string[]` *required*

Public account home page preferences.

Available options: `hide_member_count`, `hide_members_card`

**industry\_type** `string | null` *required*

Specific industry vertical for the account. See the [business types and
industries
glossary](https://docs.whop.com/api-reference/accounts/account#business-types-and-industries-glossary)
for valid values.

**invoice\_prefix** `string | null` *required*

Prefix used for account invoices.

**level** `object | null` *required*

The Whop Levels progress of the account's `owner`, counted across every account they own, so all of them show the same level. Computed only on `retrieve` and `me` for callers with `company:basic:read` scope; `null` otherwise, and when the account is not in Whop Levels.

> [accordion] Properties
>
> **account\_level** `integer` *required*
>
> This account's own level from its points alone, from `0` up to `10`. `current`
> is higher when the owner's other accounts lift it.
>
> **current** `integer` *required*
>
> The level that applies: the highest the owner reached this month or in the 5
> months before, from `0` up to `10`.
>
> **points** `integer` *required*
>
> Points earned this month by the owner and their accounts, rounded down. An
> account whose month nets below zero counts as zero.
>
> **points\_by\_source** `object` *required*
>
> This month's points, split by where they were earned. Each source is net of its refunds, so one can be negative.
>
> > [accordion] Properties
> >
> > **actions** `integer` *required*
> >
> > Points earned from eligible actions on Whop, rounded toward zero.
> >
> > **ads** `integer` *required*
> >
> > Points from paid Whop Ads charges, net of refunds: 10 for every dollar,
> > rounded toward zero.
> >
> > **cards** `integer` *required*
> >
> > Points from captured Whop Card spend, net of refunds: 10 for every dollar,
> > rounded toward zero.
> >
> > **partners** `integer` *required*
> >
> > Points from accounts the owner referred as a partner: 1 for every 3 they earn,
> > rounded toward zero.
> >
> > **trading** `integer` *required*
> >
> > Points from perpetual trading volume filled through Whop: 1 for every dollar traded, rounded toward zero.
>
> **resets\_at** `string` *required*
>
> When this month's points reset to zero, as an ISO 8601 timestamp.

**logo\_url** `string | null` *required*

Account logo image URL.

**merchant\_of\_record** `string | null` *required*

The merchant of record for the account's payments. `whop` means Whop is the merchant of record and the account can switch. `null` means Whop is the merchant of record and the account cannot switch. `pending` means the account switched and waits for approval. Checkout takes no payments until then. `required_actions` lists what the account must answer, or says that it was not approved. `account` means the account is the merchant of record. Switch with `merchant_of_record` on `PATCH /accounts/\{id}/preferences`. Whop sends `account.updated` when this value changes.

Available options: `whop`, `pending`, `account`

**metadata** `object` *required*

Arbitrary key/value metadata supplied at account creation.

**onboarding\_type** `string | null` *required*

Type of onboarding the account has completed.

Available options: `platform`, `seller`

**opengraph\_image\_url** `string | null` *required*

Account Open Graph image URL.

**opengraph\_image\_variant** `string | null` *required*

Account Open Graph image variant.

Available options: `white`, `black`, `orange`

**orchestration\_enabled** `boolean` *required*

Whether payment orchestration is enabled for this account.

**other\_business\_description** `string | null` *required*

Business type details when business\_type is `other`.

**other\_industry\_description** `string | null` *required*

Industry details when industry\_type is `other`.

**owner** `object` *required*

The single user who owns the account, whose email is the `email` above. Distinct from the `owner` role on team members, which any number of them can hold.

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

**parent\_account** `object | null` *required*

Parent account for subaccounts, or `null` for standalone accounts.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **fees** `object`
>
> Markup rates this parent charges the subaccount being read, keyed by fee type
> (for example `crypto_deposit_markup`), each with `percentage_fee` and
> `fixed_fee_usd`. Resolved with the subaccount's own overrides winning over the
> platform default.
>
> **logo\_url** `string | null` *required*
>
> Account logo image URL.
>
> **route** `string` *required*
>
> Account public route identifier.
>
> **title** `string` *required*
>
> Account display name.

**partner** `object | null`

The account's active first-tier partner. Present on retrieve responses; null when no active first-tier partner is attributed to the account. Omitted from other responses.

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
> **email** `string | null` *required*
>
> Email address for contacting the partner. Null when the partner has not added
> their own email address.
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
> **referring\_account** `object | null` *required*
>
> Business the partner referred this account on behalf of, when the referral belongs to their team. `null` for a personal referral.
>
> > [accordion] Properties
> >
> > **id** `string` *required*
> >
> > Account ID, prefixed `biz_`.
> >
> > **title** `string` *required*
> >
> > Account display name.
>
> **username** `string` *required*
>
> Public username.
>
> **whop\_partner\_verified\_at** `string | null` *required*
>
> When the user became a verified Whop Partner, as an ISO 8601 timestamp. Null if not verified.

**payment\_controls** `object | null` *required*

Payment health controls currently applied to the account. Computed only on `retrieve` and `me` for callers with `company:balance:read` scope; `null` otherwise.

> [accordion] Properties
>
> **dispute\_alert\_auto\_refund** `object` *required*
>
> Automatic refund settings for pre-chargeback dispute alerts.
>
> > [accordion] Properties
> >
> > **locked** `boolean` *required*
> >
> > Whether the account owner is prevented from changing this threshold.
> >
> > **threshold\_usd** `number | null` *required*
> >
> > Maximum dispute alert amount automatically refunded in USD. `null` when automatic refunds are disabled.
>
> **dispute\_alert\_fee\_usd** `number | null` *required*
>
> Fee charged for each dispute alert in USD. `null` when unavailable.
>
> **enforce\_3ds** `boolean` *required*
>
> Whether 3-D Secure is forced on every card payment at checkout. The account
> cannot bypass it while set.
>
> **financing\_disabled** `boolean` *required*
>
> Whether payment health controls explicitly disable financing. This is
> independent of financing approval in `capabilities.accept_bnpl_payments`.
>
> **high\_risk\_processing\_fee\_percentage** `number` *required*
>
> Additional processing fee percentage for high-risk processing.
>
> **payment\_method\_warnings** `object[]` *required*
>
> Payment methods the account has been warned are nearing their dispute-rate
> line. Each entry carries `payment_method`, `measure` (`disputes`,
> `disputes_and_tc40s` or `fraud_disputes`), `warned_at` as an ISO 8601
> timestamp, and the `rate`, `warning_rate` and `target_rate` as fractions where
> `0.01` is 1%. Empty when none are warned. A warned method is paused, and
> appears in `restricted_payment_methods`, if it is still over its target rate
> about four weeks later.
>
> **pending\_auto\_topup\_fee\_percentage** `number` *required*
>
> Percentage fee charged when pending, not-yet-settled balance is advanced to
> fund the account's cards balance, where `2` means 2%. `0` when the account is
> exempt.
>
> **pending\_balance\_delay\_days** `integer` *required*
>
> Additional days payments remain pending before becoming available.
>
> **reserve** `object` *required*
>
> Reserve currently applied to incoming payment volume.
>
> > [accordion] Properties
> >
> > **hold\_period\_days** `integer` *required*
> >
> > Number of days reserved funds are held before release.
> >
> > **percentage** `number | null` *required*
> >
> > Percentage of incoming payment volume held in reserve. `null` when no reserve is applied.
>
> **resolution\_center\_auto\_refund** `object` *required*
>
> Settings for automatic refunds of Resolution Center cases. You can change them with `PATCH /accounts/\{id}`.
>
> > [accordion] Properties
> >
> > **card\_threshold\_usd** `number | null` *required*
> >
> > Whop automatically refunds Resolution Center cases on card payments up to this
> > amount in USD. Card payments include Visa, Mastercard, American Express and
> > Discover. `null` when Whop does not automatically refund card cases. You can
> > change it with `PATCH /accounts/\{id}` while `locked` is `false`.
> >
> > **financing\_threshold\_usd** `number | null` *required*
> >
> > Whop automatically refunds Resolution Center cases on financing payments up to
> > this amount in USD. Financing payments include Klarna, Affirm, Zip, Afterpay,
> > Splitit and Sezzle. `null` when Whop does not automatically refund financing
> > cases. You can change it with `PATCH /accounts/\{id}` while `locked` is
> > `false`.
> >
> > **locked** `boolean` *required*
> >
> > Whether the thresholds are locked. While `true`, `PATCH /accounts/\{id}`
> > refuses a threshold change with a `403`. You cannot change this field.
> >
> > **message** `string | null` *required*
> >
> > Message the customer gets when Whop automatically refunds their case. `null`
> > when no message is set. You can change it with `PATCH /accounts/\{id}`. You
> > can also change it while `locked` is `true`.
> >
> > **paypal\_threshold\_usd** `number | null` *required*
> >
> > Whop automatically refunds Resolution Center cases on PayPal and Venmo payments up to this amount in USD. `null` when Whop does not automatically refund these cases. You can change it with `PATCH /accounts/\{id}` while `locked` is `false`.
>
> **restricted\_payment\_methods** `string[]` *required*
>
> Card brands blocked at checkout for this account. Empty when none are blocked. The account cannot re-enable them itself.
>
> Available options: `card_visa`, `card_mastercard`, `card_american_express`, `card_discover_global_network`, `addi`, `affirm`, `afterpay_clearpay`, `alma`, `billie`, `capchase_pay`, `claritypay`, `flex_pay`, `klarna`, `kriya`, `kueski`, `mondu`, `oney`, `scalapay`, `sequra`, `sezzle`, `splitit`, `sunbit`, `tabby`, `tamara`, `zip`, `card_installments_three`, `card_installments_six`, `card_installments_twelve`, `oney_3x`, `oney_4x`, `klarna_pay_now`, `us_bank_account`, `sepa_debit`, `bacs_debit`, `au_becs_debit`, `acss_debit`, `pay_by_bank`, `payto`, `paypal`, `cashapp`, `venmo`, `amazon_pay`, `link`, `wechat_pay`
>
> **undated\_pending\_reason** `string | null` *required*
>
> Why pending funds without a settlement date aren't moving yet. `kyc_incomplete`, `pending_information_request`, and `update_payout_profile` are things the merchant can act on. `compliance_review` means a verification the merchant already submitted is being reviewed, so these funds stay pending until it clears. `withdrawals_disabled` means Whop has blocked withdrawals, so these funds cannot become available. `null` when there's no reason to show — still clearing, or held for a reason that isn't named here.
>
> Available options: `kyc_incomplete`, `pending_information_request`, `update_payout_profile`, `compliance_review`, `withdrawals_disabled`
>
> **withdrawal\_schedule** `object` *required*
>
> How the account's balance automatically withdraws.
>
> > [accordion] Properties
> >
> > **day** `integer | null` *required*
> >
> > Day the automatic withdrawal runs on: 0-6 (Sunday-Saturday) for `weekly`, 1-31
> > for `monthly`. `null` for `manual` and `daily`.
> >
> > **frequency** `string` *required*
> >
> > How often the account's balance automatically withdraws.
> >
> > Available options: `manual`, `daily`, `weekly`, `monthly`
> >
> > **next\_payout\_date** `string | null` *required*
> >
> > Next date the automatic withdrawal is scheduled to run, as an ISO 8601 date. `null` for `manual` and `daily`, where no single next date applies.

**payment\_method\_configuration** `object | null` *required*

The account's default payment methods at checkout. A variant's or checkout configuration's own `payment_method_configuration` takes precedence. `null` when the account uses Whop's platform defaults.

> [accordion] Properties
>
> **disabled** `string[]` *required*
>
> Payment methods this checkout withholds, even when the defaults would offer
> them.
>
> **enabled** `string[]` *required*
>
> Payment methods this checkout offers on top of whatever the defaults provide.
>
> **include\_platform\_defaults** `boolean` *required*
>
> Whether Whop's default set is the starting point. When `false`, only `enabled` is offered.

**platform\_credits** `object[]` *required*

Whop-issued platform credits per currency, read from the balance cache. Credits can only be spent on eligible Whop products such as Whop Ads, can never be withdrawn, and are not included in `balances` or `total_usd`. Empty when the account holds none or `total_usd` is `null`.

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

**privacy\_policy** `object | null` *required*

The account's privacy policy document, or `null` if they have not published one.

> [accordion] Properties
>
> **id** `string` *required*
>
> The file's ID, prefixed `file_`.
>
> **blurhash** `string | null` *required*
>
> BlurHash placeholder for an image or video. Null when unavailable or removed
> by moderation.
>
> **content\_type** `string | null` *required*
>
> The original file's MIME type, for example `application/pdf`. It is kept when
> moderation removes the file.
>
> **created\_at** `string` *required*
>
> When the file was created, as an ISO 8601 timestamp.
>
> **duration\_seconds** `number | null` *required*
>
> Audio or video duration in seconds. Null when unavailable or removed by
> moderation.
>
> **filename** `string | null` *required*
>
> The original filename, including its extension.
>
> **height** `integer | null` *required*
>
> Image or video height in pixels. Null when unavailable or removed by
> moderation.
>
> **multipart\_chunk\_size** `integer | null`
>
> The byte size each part (except the last) must be. Present only on create, and
> only for multipart uploads.
>
> **multipart\_upload\_id** `string | null`
>
> The ID of the multipart upload, passed back to `complete`. Present only on
> create, and only for multipart uploads.
>
> **multipart\_upload\_urls** `object[] | null`
>
> The presigned URL for each part. Present only on create, and only for multipart uploads.
>
> > [accordion] Properties
> >
> > **part\_number** `integer` *required*
> >
> > The 1-based index of this part within the multipart upload.
> >
> > **url** `string` *required*
> >
> > The presigned URL to PUT this part's bytes to.
>
> **object** `string` *required*
>
> The type of this object, always `file`.
>
> **optimized\_url** `string | null` *required*
>
> URL to the optimized file. `null` when no optimized version is available.
>
> **removed\_by\_moderation** `boolean` *required*
>
> Whether moderation removed the file. When `true`, `url` serves a replacement
> image. `filename`, `size` and `content_type` still describe the original file.
>
> **size** `integer | null` *required*
>
> The file size in bytes. `null` until the upload has finished.
>
> **upload\_headers** `object`
>
> Headers to send with the upload PUT. Present only on create.
>
> **upload\_status** `string` *required*
>
> Where the file is in its upload lifecycle.
>
> Available options: `pending`, `processing`, `ready`, `failed`
>
> **upload\_url** `string | null`
>
> Presigned URL to PUT the file's bytes to. Present only on create, and only for
> single-part uploads.
>
> **url** `string | null` *required*
>
> A URL to download the file. Public files get a permanent CDN URL, and private
> files get a signed URL that expires. `null` until the upload has finished.
> When moderation removes the file, a replacement image URL.
>
> **visibility** `string` *required*
>
> `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
>
> Available options: `public`, `private`
>
> **waveform\_url** `string | null` *required*
>
> Audio waveform URL. Null when unavailable or removed by moderation.
>
> **width** `integer | null` *required*
>
> Image or video width in pixels. Null when unavailable or removed by moderation.

**product\_tax\_code** `object | null` *required*

Tax classification code applied by default to the account's products, with
`id`, `name`, and `product_type`. `null` when no default is set.

**recommended\_actions** `object[] | null` *required*

DEPRECATED: Use the `GET /economic_intelligence?account_id=\{account_id}` endpoint instead.

> [accordion] Properties
>
> **action** `string` *required*
>
> The recommendation; new values may be added, so handle unknown actions gracefully
>
> Available options: `theme_business`, `create_product`, `create_plan`, `verify_identity`, `connect_affiliate_program`, `create_promotion`, `migrate_from_stripe`, `accept_first_payment`, `launch_first_ad`, `launch_draft_campaign`, `increase_ad_budget`, `refresh_ad_creatives`, `fix_ad_billing`, `exclude_customers_from_ads`, `retarget_abandoned_checkouts`, `fix_funnel_dropoff`, `invite_team_member`, `enable_tax_collection`, `create_card`, `apply_for_financing`
>
> **blocked\_capabilities** `string[]` *required*
>
> Capabilities this would unlock, or empty
>
> **cta** `string` *required*
>
> The URL the call-to-action links to
>
> **cta\_label** `string` *required*
>
> Button label
>
> **description** `string` *required*
>
> Supporting copy, or empty
>
> **icon\_url** `string | null` *required*
>
> Illustration icon URL, or `null`
>
> **impact\_score** `integer | null` *required*
>
> Estimated impact from 0-100, or `null` when not ranked
>
> **reasoning** `string | null` *required*
>
> Why this action was recommended, or `null`
>
> **status** `string` *required*
>
> Always optional — never blocking
>
> Available options: `optional`
>
> **title** `string` *required*
>
> Headline for the recommendation

**require\_2fa** `boolean` *required*

Whether authorized users must enable two-factor authentication.

**required\_actions** `object[] | null` *required*

Actions the account owner must take to unblock capabilities like payouts and card spend, ordered by display priority. Computed only on `retrieve` and `me` for callers with `company:balance:read` scope; `null` otherwise.

> [accordion] Properties
>
> **action** `string` *required*
>
> What the holder must do; new values may be added, so handle unknown actions gracefully
>
> Available options: `deposit_funds`, `review_held_payments`, `submit_information_request`, `update_automatic_withdrawal_method`, `reauthorize_payout_methods`, `update_payout_method`, `update_payout_profile`, `card_usage_review`, `verify_identity`, `scale_account_setup`, `sign_formation_documents`, `configure_tax_remitter`, `add_vat_registration`, `confirm_tax_details`, `accept_payout_terms`, `enable_two_factor_authentication`, `respond_to_abuse_report`
>
> **blocked\_capabilities** `string[]` *required*
>
> Capabilities gated until this is resolved
>
> **cta** `string | null` *required*
>
> The URL the call-to-action links to, or null when there is no button
>
> **cta\_label** `string` *required*
>
> Button label, or empty when there is no button
>
> **description** `string` *required*
>
> Supporting copy, or empty
>
> **icon\_url** `string | null` *required*
>
> The URL of the action's illustration icon, or null if it has none
>
> **status** `string` *required*
>
> required (act now) or pending (under review)
>
> Available options: `required`, `pending`
>
> **title** `string` *required*
>
> Headline for the action

**return\_policy** `object | null` *required*

The account's return policy document, or `null` if they have not published one.

> [accordion] Properties
>
> **id** `string` *required*
>
> The file's ID, prefixed `file_`.
>
> **blurhash** `string | null` *required*
>
> BlurHash placeholder for an image or video. Null when unavailable or removed
> by moderation.
>
> **content\_type** `string | null` *required*
>
> The original file's MIME type, for example `application/pdf`. It is kept when
> moderation removes the file.
>
> **created\_at** `string` *required*
>
> When the file was created, as an ISO 8601 timestamp.
>
> **duration\_seconds** `number | null` *required*
>
> Audio or video duration in seconds. Null when unavailable or removed by
> moderation.
>
> **filename** `string | null` *required*
>
> The original filename, including its extension.
>
> **height** `integer | null` *required*
>
> Image or video height in pixels. Null when unavailable or removed by
> moderation.
>
> **multipart\_chunk\_size** `integer | null`
>
> The byte size each part (except the last) must be. Present only on create, and
> only for multipart uploads.
>
> **multipart\_upload\_id** `string | null`
>
> The ID of the multipart upload, passed back to `complete`. Present only on
> create, and only for multipart uploads.
>
> **multipart\_upload\_urls** `object[] | null`
>
> The presigned URL for each part. Present only on create, and only for multipart uploads.
>
> > [accordion] Properties
> >
> > **part\_number** `integer` *required*
> >
> > The 1-based index of this part within the multipart upload.
> >
> > **url** `string` *required*
> >
> > The presigned URL to PUT this part's bytes to.
>
> **object** `string` *required*
>
> The type of this object, always `file`.
>
> **optimized\_url** `string | null` *required*
>
> URL to the optimized file. `null` when no optimized version is available.
>
> **removed\_by\_moderation** `boolean` *required*
>
> Whether moderation removed the file. When `true`, `url` serves a replacement
> image. `filename`, `size` and `content_type` still describe the original file.
>
> **size** `integer | null` *required*
>
> The file size in bytes. `null` until the upload has finished.
>
> **upload\_headers** `object`
>
> Headers to send with the upload PUT. Present only on create.
>
> **upload\_status** `string` *required*
>
> Where the file is in its upload lifecycle.
>
> Available options: `pending`, `processing`, `ready`, `failed`
>
> **upload\_url** `string | null`
>
> Presigned URL to PUT the file's bytes to. Present only on create, and only for
> single-part uploads.
>
> **url** `string | null` *required*
>
> A URL to download the file. Public files get a permanent CDN URL, and private
> files get a signed URL that expires. `null` until the upload has finished.
> When moderation removes the file, a replacement image URL.
>
> **visibility** `string` *required*
>
> `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
>
> Available options: `public`, `private`
>
> **waveform\_url** `string | null` *required*
>
> Audio waveform URL. Null when unavailable or removed by moderation.
>
> **width** `integer | null` *required*
>
> Image or video width in pixels. Null when unavailable or removed by moderation.

**rewards** `object[]`

Business rewards attached through this account's active referral link, with account-specific progress and ledger status. Present on retrieve responses. Empty without both balance and stats read access.

> [accordion] Properties
>
> **id** `string` *required*
>
> Reward definition ID, prefixed `prwd_`. Progress and status apply to the
> containing account.
>
> **qualification\_amount** `object` *required*
>
> Qualifying USD volume required to earn this reward.
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
> **qualification\_progress** `object` *required*
>
> Qualifying USD volume for the reward’s activity accumulated by this account since attribution, calculated using the fulfillment rules.
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
> **qualification\_type** `string` *required*
>
> Activity that qualifies this account for the reward.
>
> Available options: `sales`, `ad_spend`
>
> **reward\_amount** `object` *required*
>
> USD amount credited for this reward. Uses the saved grant amount once fulfillment has started.
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
> **status** `string` *required*
>
> This account's reward state. Credited requires a posted ledger entry; processing includes a met requirement awaiting fulfillment. Reversing and reversed reflect a subsequent reward reversal.
>
> Available options: `in_progress`, `processing`, `credited`, `reversing`, `reversed`, `unavailable`

**route** `string` *required*

Account public route identifier.

**send\_customer\_emails** `boolean` *required*

Whether Whop sends transactional emails to customers on behalf of this
account.

**shipping\_policy** `object | null` *required*

The account's shipping policy document, or `null` if they have not published one.

> [accordion] Properties
>
> **id** `string` *required*
>
> The file's ID, prefixed `file_`.
>
> **blurhash** `string | null` *required*
>
> BlurHash placeholder for an image or video. Null when unavailable or removed
> by moderation.
>
> **content\_type** `string | null` *required*
>
> The original file's MIME type, for example `application/pdf`. It is kept when
> moderation removes the file.
>
> **created\_at** `string` *required*
>
> When the file was created, as an ISO 8601 timestamp.
>
> **duration\_seconds** `number | null` *required*
>
> Audio or video duration in seconds. Null when unavailable or removed by
> moderation.
>
> **filename** `string | null` *required*
>
> The original filename, including its extension.
>
> **height** `integer | null` *required*
>
> Image or video height in pixels. Null when unavailable or removed by
> moderation.
>
> **multipart\_chunk\_size** `integer | null`
>
> The byte size each part (except the last) must be. Present only on create, and
> only for multipart uploads.
>
> **multipart\_upload\_id** `string | null`
>
> The ID of the multipart upload, passed back to `complete`. Present only on
> create, and only for multipart uploads.
>
> **multipart\_upload\_urls** `object[] | null`
>
> The presigned URL for each part. Present only on create, and only for multipart uploads.
>
> > [accordion] Properties
> >
> > **part\_number** `integer` *required*
> >
> > The 1-based index of this part within the multipart upload.
> >
> > **url** `string` *required*
> >
> > The presigned URL to PUT this part's bytes to.
>
> **object** `string` *required*
>
> The type of this object, always `file`.
>
> **optimized\_url** `string | null` *required*
>
> URL to the optimized file. `null` when no optimized version is available.
>
> **removed\_by\_moderation** `boolean` *required*
>
> Whether moderation removed the file. When `true`, `url` serves a replacement
> image. `filename`, `size` and `content_type` still describe the original file.
>
> **size** `integer | null` *required*
>
> The file size in bytes. `null` until the upload has finished.
>
> **upload\_headers** `object`
>
> Headers to send with the upload PUT. Present only on create.
>
> **upload\_status** `string` *required*
>
> Where the file is in its upload lifecycle.
>
> Available options: `pending`, `processing`, `ready`, `failed`
>
> **upload\_url** `string | null`
>
> Presigned URL to PUT the file's bytes to. Present only on create, and only for
> single-part uploads.
>
> **url** `string | null` *required*
>
> A URL to download the file. Public files get a permanent CDN URL, and private
> files get a signed URL that expires. `null` until the upload has finished.
> When moderation removes the file, a replacement image URL.
>
> **visibility** `string` *required*
>
> `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
>
> Available options: `public`, `private`
>
> **waveform\_url** `string | null` *required*
>
> Audio waveform URL. Null when unavailable or removed by moderation.
>
> **width** `integer | null` *required*
>
> Image or video width in pixels. Null when unavailable or removed by moderation.

**show\_joined\_whops** `boolean` *required*

Whether the account appears in joined whops on other accounts.

**show\_reviews\_dtc** `boolean` *required*

Whether reviews are displayed on direct-to-consumer product pages.

**show\_user\_directory** `boolean` *required*

Whether the account shows users in the user directory.

**social\_links** `object[]` *required*

Account social links.

> [accordion] Properties
>
> **id** `string` *required*
>
> The ID of the social link
>
> **title** `string | null` *required*
>
> The optional display title for the social link
>
> **url** `string` *required*
>
> The social link URL
>
> **website** `string` *required*
>
> The social platform for this link
>
> Available options: `x`, `instagram`, `facebook`, `tiktok`, `youtube`, `linkedin`, `twitch`, `website`, `custom`

**statement\_descriptor** `string | null` *required*

The text on buyers' card statements for this account's payments, up to 22
characters. While Whop is the merchant of record, it starts with `WHOP*`. Whop
drops the `WHOP*` prefix when the account is its own merchant of record and
can take card payments. It keeps the prefix if fewer than five characters or
no letters would remain. A product or payment descriptor takes precedence.
`null` uses a descriptor made from the account's title.

**status** `string | null` *required*

Whether the account can operate on Whop: `active` or `suspended`. Computed on
`list`, `retrieve`, `me`, and `suspend`; `null` otherwise.

**status\_reason** `string | null` *required*

Why the account was suspended, as a label such as `Suspended - Fraudulent
      	activity`. `Suspended` when no reason is recorded. Computed on `retrieve`,
`me`, and `suspend`. `null` otherwise, or when `status` is not `suspended`.

**store\_page\_config** `object` *required*

Account store page display configuration.

> [accordion] Properties
>
> **accent\_color** `string | null` *required*
>
> Accent color used on the account store page.
>
> Available options: `ruby`, `tomato`, `red`, `crimson`, `pink`, `plum`, `purple`, `violet`, `iris`, `cyan`, `teal`, `jade`, `green`, `grass`, `brown`, `blue`, `orange`, `indigo`, `sky`, `mint`, `yellow`, `amber`, `lime`, `lemon`, `magenta`, `gold`, `bronze`, `gray`
>
> **layout** `string | null` *required*
>
> Layout used on the account store page.
>
> Available options: `featured`, `compact`
>
> **profile\_variant** `string | null` *required*
>
> Profile presentation used on the account store page.
>
> Available options: `personal`, `business`
>
> **whop\_affiliate\_link** `boolean` *required*
>
> Whether the account store page shows a Whop affiliate link.

**target\_audience** `string | null` *required*

Target audience for this account.

**tax\_collection\_enabled\_states** `string[]` *required*

US state codes (of the 50 states plus `DC`) where the account collects tax:
the full set when Whop remits (`tax_remitted_by` `whop`), the configured
subset when the account self-remits (`self`), and empty when neither. On
update, send the complete list to replace it (only allowed when `self`).

**tax\_identifiers** `object[]` *required*

Account tax/VAT registrations. Empty when none are set.

> [accordion] Properties
>
> **id** `string` *required*
>
> Tax identifier ID.
>
> **tax\_id\_type** `string` *required*
>
> Tax ID type.
>
> Available options: `ad_nrt`, `ao_tin`, `ar_cuit`, `al_tin`, `am_tin`, `aw_tin`, `au_abn`, `au_arn`, `eu_vat`, `az_tin`, `bs_tin`, `bh_vat`, `bd_bin`, `bb_tin`, `by_tin`, `bj_ifu`, `bo_tin`, `ba_tin`, `br_cnpj`, `br_cpf`, `bg_uic`, `bf_ifu`, `kh_tin`, `cm_niu`, `ca_bn`, `ca_gst_hst`, `ca_pst_bc`, `ca_pst_mb`, `ca_pst_sk`, `ca_qst`, `cv_nif`, `cl_tin`, `cn_tin`, `co_nit`, `cd_nif`, `cr_tin`, `hr_oib`, `do_rcn`, `ec_ruc`, `eg_tin`, `sv_nit`, `et_tin`, `eu_oss_vat`, `ge_vat`, `gh_tin`, `de_stn`, `gb_vat`, `gn_nif`, `hk_br`, `hu_tin`, `is_vat`, `in_gst`, `id_npwp`, `il_vat`, `jp_cn`, `jp_rn`, `jp_trn`, `kz_bin`, `ke_pin`, `kg_tin`, `la_tin`, `li_uid`, `li_vat`, `my_frp`, `my_itn`, `my_sst`, `mr_nif`, `mx_rfc`, `md_vat`, `me_pib`, `ma_vat`, `np_pan`, `nz_gst`, `ng_tin`, `mk_vat`, `no_vat`, `no_voec`, `om_vat`, `pe_ruc`, `ph_tin`, `pl_nip`, `ro_tin`, `ru_inn`, `ru_kpp`, `sa_vat`, `sn_ninea`, `rs_pib`, `sg_gst`, `sg_uen`, `si_tin`, `za_vat`, `kr_brn`, `es_cif`, `ch_uid`, `ch_vat`, `tw_vat`, `tj_tin`, `tz_vat`, `th_vat`, `tr_tin`, `ug_tin`, `ua_vat`, `ae_trn`, `us_ein`, `uy_ruc`, `uz_tin`, `uz_vat`, `ve_rif`, `vn_tin`, `zm_tin`, `zw_tin`, `sr_fin`, `xi_vat`
>
> **tax\_id\_value** `string` *required*
>
> Tax ID value, for example `DE123456789`. See the [tax IDs guide](https://docs.whop.com/developer/guides/tax-ids.md).

**tax\_remitted\_by** `string | null` *required*

Who calculates and remits tax for the account. `whop` means Whop calculates and remits. `self` means Whop calculates, and the account collects and remits. `none` means neither, and the account is responsible. New businesses start on `whop`, and new connected accounts start on `none`. `null` means the account has no setting yet.

Available options: `whop`, `self`, `none`

**tax\_type** `string | null` *required*

How tax is applied to the account's prices: `inclusive` (tax included in the listed price) or `exclusive` (tax added on top). Defaults to `exclusive` when unset; `null` only when the account has no payment connection.

Available options: `inclusive`, `exclusive`

**terms\_of\_service** `object | null` *required*

The account's terms of service document, or `null` if they have not published one.

> [accordion] Properties
>
> **id** `string` *required*
>
> The file's ID, prefixed `file_`.
>
> **blurhash** `string | null` *required*
>
> BlurHash placeholder for an image or video. Null when unavailable or removed
> by moderation.
>
> **content\_type** `string | null` *required*
>
> The original file's MIME type, for example `application/pdf`. It is kept when
> moderation removes the file.
>
> **created\_at** `string` *required*
>
> When the file was created, as an ISO 8601 timestamp.
>
> **duration\_seconds** `number | null` *required*
>
> Audio or video duration in seconds. Null when unavailable or removed by
> moderation.
>
> **filename** `string | null` *required*
>
> The original filename, including its extension.
>
> **height** `integer | null` *required*
>
> Image or video height in pixels. Null when unavailable or removed by
> moderation.
>
> **multipart\_chunk\_size** `integer | null`
>
> The byte size each part (except the last) must be. Present only on create, and
> only for multipart uploads.
>
> **multipart\_upload\_id** `string | null`
>
> The ID of the multipart upload, passed back to `complete`. Present only on
> create, and only for multipart uploads.
>
> **multipart\_upload\_urls** `object[] | null`
>
> The presigned URL for each part. Present only on create, and only for multipart uploads.
>
> > [accordion] Properties
> >
> > **part\_number** `integer` *required*
> >
> > The 1-based index of this part within the multipart upload.
> >
> > **url** `string` *required*
> >
> > The presigned URL to PUT this part's bytes to.
>
> **object** `string` *required*
>
> The type of this object, always `file`.
>
> **optimized\_url** `string | null` *required*
>
> URL to the optimized file. `null` when no optimized version is available.
>
> **removed\_by\_moderation** `boolean` *required*
>
> Whether moderation removed the file. When `true`, `url` serves a replacement
> image. `filename`, `size` and `content_type` still describe the original file.
>
> **size** `integer | null` *required*
>
> The file size in bytes. `null` until the upload has finished.
>
> **upload\_headers** `object`
>
> Headers to send with the upload PUT. Present only on create.
>
> **upload\_status** `string` *required*
>
> Where the file is in its upload lifecycle.
>
> Available options: `pending`, `processing`, `ready`, `failed`
>
> **upload\_url** `string | null`
>
> Presigned URL to PUT the file's bytes to. Present only on create, and only for
> single-part uploads.
>
> **url** `string | null` *required*
>
> A URL to download the file. Public files get a permanent CDN URL, and private
> files get a signed URL that expires. `null` until the upload has finished.
> When moderation removes the file, a replacement image URL.
>
> **visibility** `string` *required*
>
> `public` files are served from an unsigned CDN URL. `private` files are served from a signed URL that expires.
>
> Available options: `public`, `private`
>
> **waveform\_url** `string | null` *required*
>
> Audio waveform URL. Null when unavailable or removed by moderation.
>
> **width** `integer | null` *required*
>
> Image or video width in pixels. Null when unavailable or removed by moderation.

**three\_ds\_level** `string | null` *required*

3D Secure behavior for supported on-session card payments. `mandate_challenge` requires a 3DS challenge before payment processing; `mandate_if_required` mandates a challenge only when the payment processor requires it; `frictionless_if_required` uses the regular frictionless 3DS flow. Payments of \$1,000 or more use `mandate_if_required` unless `mandate_challenge` is selected. Risk and authentication recovery requirements can override the preference. `null` uses the standard checkout flow.

Available options: `mandate_challenge`, `mandate_if_required`, `frictionless_if_required`

**title** `string` *required*

Account display name.

**total\_earned\_usd** `number | null` *required*

Account lifetime sales, normalized to USD. Computed only on `retrieve` and
`me` for callers with `stats:read` scope; `null` otherwise.

**total\_usd** `string | null` *required*

Total USD value across balances with known exchange rates. Computed only on
single-account reads (`retrieve` and `me`); `null` on list responses, writes,
missing balance-read permission, or unavailable balance source.

**trading** `trading_account | null` *required*

The trading account address, a snapshot of its margin and open positions, and its WebSocket subscriptions. Opt in with `include_trading=true` on single-account reads. `null` otherwise, without trading permission, or without an Ethereum wallet.

> [accordion] Properties
>
> **id** `string` *required*
>
> The Whop wallet ID backing this trading account, prefixed `cwal_`.
>
> **account\_id** `string | null` *required*
>
> The account that owns this trading account, prefixed `biz_`. `null` when a
> user owns it.
>
> **hyperliquid** `object | null` *required*
>
> Hyperliquid-specific state. Present when `provider` is `hyperliquid`, otherwise `null`.
>
> > [accordion] Properties
> >
> > **address** `string` *required*
> >
> > Lowercase wallet address that holds the Hyperliquid account.
> >
> > **depositable** `string | null` *required*
> >
> > USDT0 in the Whop wallet on Plasma that a `deposit` trade can move into this
> > account, as a decimal string. `null` when the chain could not be read.
> >
> > **margin\_summary** `object | null` *required*
> >
> > Account value and margin across the main exchange and the `xyz` markets, read from Hyperliquid at most once a second. `null` when Hyperliquid could not be read. Use `websocket_subscriptions` for live updates.
> >
> > > [accordion] Properties
> > >
> > > **account\_value** `object` *required*
> > >
> > > Total account value in USD, including unrealized profit and loss.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **total\_margin\_used** `object` *required*
> > >
> > > Margin allocated across open positions, in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **total\_position\_notional** `object` *required*
> > >
> > > Combined notional value of open positions, in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **withdrawable** `object` *required*
> > >
> > > USD that can be withdrawn now without closing positions.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> >
> > **positions** `trading_position[] | null` *required*
> >
> > Open perpetual positions across the main exchange and the `xyz` markets, read from Hyperliquid at most once a second. `null` when Hyperliquid could not be read. Use `websocket_subscriptions` for live updates.
> >
> > > [accordion] Properties
> > >
> > > **id** `string` *required*
> > >
> > > Trading position ID, prefixed `trdpos_`. Stable for a market within one
> > > trading account.
> > >
> > > **entry\_price** `object | null` *required*
> > >
> > > Average entry price in USD, or `null` when the provider omits it.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **hyperliquid** `object | null` *required*
> > >
> > > Hyperliquid perpetual details. Present on Hyperliquid positions, otherwise `null`.
> > >
> > > > [accordion] Properties
> > > >
> > > > **cumulative\_funding** `object` *required*
> > > >
> > > > Funding paid on the position over several windows, in USD.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **all\_time** `object | null` *required*
> > > > >
> > > > > Funding paid on this market across the account's history, in USD, or `null` when unavailable.
> > > > >
> > > > > > [accordion] Properties
> > > > > >
> > > > > > **amount** `string` *required*
> > > > > >
> > > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > > >
> > > > > > **currency** `string` *required*
> > > > > >
> > > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > > >
> > > > > > **decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > > itself runs at.
> > > > > >
> > > > > > **display\_decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > > >
> > > > > **since\_change** `object | null` *required*
> > > > >
> > > > > Funding paid since the position size last changed, in USD, or `null` when unavailable.
> > > > >
> > > > > > [accordion] Properties
> > > > > >
> > > > > > **amount** `string` *required*
> > > > > >
> > > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > > >
> > > > > > **currency** `string` *required*
> > > > > >
> > > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > > >
> > > > > > **decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > > itself runs at.
> > > > > >
> > > > > > **display\_decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > > >
> > > > > **since\_open** `object | null` *required*
> > > > >
> > > > > Funding paid since the position opened, in USD, or `null` when unavailable.
> > > > >
> > > > > > [accordion] Properties
> > > > > >
> > > > > > **amount** `string` *required*
> > > > > >
> > > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > > >
> > > > > > **currency** `string` *required*
> > > > > >
> > > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > > >
> > > > > > **decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > > itself runs at.
> > > > > >
> > > > > > **display\_decimals** `integer` *required*
> > > > > >
> > > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > >
> > > > **leverage** `object` *required*
> > > >
> > > > Margin mode and multiplier for the position.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **type** `string` *required*
> > > > >
> > > > > `cross` shares margin across positions. `isolated` limits margin to this position.
> > > > >
> > > > > Available options: `cross`, `isolated`
> > > > >
> > > > > **value** `integer` *required*
> > > > >
> > > > > Multiplier applied to the position's margin, such as `10` for 10x.
> > > >
> > > > **liquidation\_price** `object | null` *required*
> > > >
> > > > Estimated liquidation price in USD, or `null` when Hyperliquid reports none.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **amount** `string` *required*
> > > > >
> > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > >
> > > > > **currency** `string` *required*
> > > > >
> > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > >
> > > > > **decimals** `integer` *required*
> > > > >
> > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > itself runs at.
> > > > >
> > > > > **display\_decimals** `integer` *required*
> > > > >
> > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > >
> > > > **margin\_used** `object` *required*
> > > >
> > > > Margin allocated to the position, in USD.
> > > >
> > > > > [accordion] Properties
> > > > >
> > > > > **amount** `string` *required*
> > > > >
> > > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > > >
> > > > > **currency** `string` *required*
> > > > >
> > > > > Three-letter ISO 4217 currency code, lowercase.
> > > > >
> > > > > **decimals** `integer` *required*
> > > > >
> > > > > How many decimal places the amount carries. This is the precision the charge
> > > > > itself runs at.
> > > > >
> > > > > **display\_decimals** `integer` *required*
> > > > >
> > > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > > >
> > > > **return\_on\_equity** `string` *required*
> > > >
> > > > Return on equity as a decimal ratio string, such as `0.1` for 10%.
> > >
> > > **market** `string` *required*
> > >
> > > Market symbol on the provider, such as `ETH`.
> > >
> > > **object** `string` *required*
> > >
> > > **position\_value** `object` *required*
> > >
> > > Current position value in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **side** `string` *required*
> > >
> > > Available options: `long`, `short`
> > >
> > > **size** `string` *required*
> > >
> > > Absolute position size as a decimal string.
> > >
> > > **unrealized\_pnl** `object` *required*
> > >
> > > Unrealized profit or loss in USD. Negative for a loss.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> >
> > **websocket\_subscriptions** `object[]` *required*
> >
> > Subscriptions for live account updates. Send each `message` unchanged after connecting to `websocket_url`.
> >
> > > [accordion] Properties
> > >
> > > **channel** `string` *required*
> > >
> > > The live update stream this subscription opens.
> > >
> > > Available options: `clearinghouse_state`, `open_orders`, `order_updates`, `user_fills`, `user_events`
> > >
> > > **message** `string` *required*
> > >
> > > JSON subscription message to send unchanged over the Hyperliquid WebSocket.
> >
> > **websocket\_url** `string` *required*
> >
> > Hyperliquid WebSocket URL to connect to directly for live updates.
>
> **object** `string` *required*
>
> **provider** `string` *required*
>
> Trading venue that holds the positions and orders.
>
> Available options: `hyperliquid`
>
> **spot** `object | null` *required*
>
> Spot holdings in the Whop wallet with their entry price and profit and loss. `null` when it could not be read.
>
> > [accordion] Properties
> >
> > **holdings** `object[]` *required*
> >
> > Each spot asset the wallet still holds, with its average entry price and profit and loss. Cost basis counts the oldest units as sold first and includes assets that arrived from outside Whop.
> >
> > > [accordion] Properties
> > >
> > > **average\_entry\_price** `object | null` *required*
> > >
> > > The average price paid per unit of what is still held, in USD, counting the oldest units as sold first. `null` when it is not known.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **cost\_basis** `object` *required*
> > >
> > > What was paid in USD for the units still held.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **market** `string` *required*
> > >
> > > The spot market's `id` from `GET /trades/markets`, such as `BTC`, `XAUT`, or a
> > > tokenized stock's ticker such as `AAPL`.
> > >
> > > **realized\_pnl** `object` *required*
> > >
> > > Gains and losses already locked in by selling, in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **unrealized\_pnl** `object` *required*
> > >
> > > The value of the units still held minus their `cost_basis`, in USD.
> > >
> > > > [accordion] Properties
> > > >
> > > > **amount** `string` *required*
> > > >
> > > > The amount in major units, as an exact decimal string. For example, `"10.00"`
> > > > is ten dollars. It is a string so that no float rounds it in transit.
> > > >
> > > > **currency** `string` *required*
> > > >
> > > > Three-letter ISO 4217 currency code, lowercase.
> > > >
> > > > **decimals** `integer` *required*
> > > >
> > > > How many decimal places the amount carries. This is the precision the charge
> > > > itself runs at.
> > > >
> > > > **display\_decimals** `integer` *required*
> > > >
> > > > How many decimal places to SHOW. Usually equal to `decimals`, and deliberately not always: COP is charged in centavos but written in whole pesos, so it is `2` and `0`. Format the number in your own locale using this.
> > >
> > > **unrealized\_pnl\_percentage** `number | null` *required*
> > >
> > > `unrealized_pnl` as a percentage of `cost_basis`, where `2` means up 2%. `null` when it is not known.
>
> **user\_id** `string | null` *required*
>
> The user who owns this trading account, prefixed `user_`. `null` when an account owns it.

**use\_logo\_as\_opengraph\_image\_fallback** `boolean` *required*

Whether the account uses its logo as the fallback Open Graph image.

**verification** `object` *required*

Account identity verification status for the `individual` (KYC) and `business`
(KYB) profiles. Each is `null` until created, otherwise a `status` of
`not_started`, `pending`, `manual_review`, `approved`, or `rejected`.

**volume\_l30d\_usd** `number | null` *required*

Volume through the account in the last 30 days (GMV from paid receipts),
normalized to USD. Computed only on `list` for callers with `stats:read` on
the account, and `null` otherwise.

**volume\_usd** `number | null` *required*

Lifetime volume through the account (sales plus transfers received),
normalized to USD. Computed only on `list` for callers with `stats:read` on
the account, and `null` otherwise.

**wallet** `object | null` *required*

Account primary crypto wallet, or `null` if none has been provisioned.

> [accordion] Properties
>
> **id** `string` *required*
>
> Wallet ID, prefixed `wallet_`.
>
> **address** `string` *required*
>
> The on-chain address of the wallet
>
> **network** `string` *required*
>
> The blockchain network the wallet lives on
>
> Available options: `solana`, `ethereum`, `bitcoin`

**website** `string | null` *required*

The account's business website URL, or `null` if none has been provided.
Setting it also adds a `website` entry to `social_links`.

**whop\_partner\_enabled\_at** `string | null`

When the account turned on Whop Partner mode, as an ISO 8601 timestamp.
Present on single-account reads and update responses; `null` until the account
turns Whop Partner mode on by sending `whop_partner_enabled: true` on update.

```json Account theme={null}
{
	"id": "biz_xxxxxxxxxxxxx",
	"trading": {
		"id": "cwal_xxxxxxxxxxxxx",
		"object": "trading_account",
		"account_id": "biz_xxxxxxxxxxxxx",
		"user_id": null,
		"provider": "hyperliquid",
		"hyperliquid": {
			"address": "0x1234567890abcdef1234567890abcdef12345678",
			"websocket_url": "wss://api.hyperliquid.xyz/ws",
			"websocket_subscriptions": [
				{
					"channel": "clearinghouse_state",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"clearinghouseState\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				},
				{
					"channel": "open_orders",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"openOrders\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				},
				{
					"channel": "order_updates",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"orderUpdates\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				},
				{
					"channel": "user_fills",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"userFills\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				},
				{
					"channel": "user_events",
					"message": "{\"method\":\"subscribe\",\"subscription\":{\"type\":\"userEvents\",\"user\":\"0x1234567890abcdef1234567890abcdef12345678\"}}"
				}
			],
			"depositable": "1250.50",
			"margin_summary": {
				"account_value": {
					"currency": "usd",
					"amount": "1520.50",
					"decimals": 2,
					"display_decimals": 2
				},
				"total_position_notional": {
					"currency": "usd",
					"amount": "4350.00",
					"decimals": 2,
					"display_decimals": 2
				},
				"total_margin_used": {
					"currency": "usd",
					"amount": "435.00",
					"decimals": 2,
					"display_decimals": 2
				},
				"withdrawable": {
					"currency": "usd",
					"amount": "500.00",
					"decimals": 2,
					"display_decimals": 2
				}
			},
			"positions": [
				{
					"id": "trdpos_0123456789abcdef",
					"object": "trading_position",
					"market": "ETH",
					"side": "short",
					"size": "1.5",
					"entry_price": {
						"currency": "usd",
						"amount": "3000.00",
						"decimals": 2,
						"display_decimals": 2
					},
					"position_value": {
						"currency": "usd",
						"amount": "4350.00",
						"decimals": 2,
						"display_decimals": 2
					},
					"unrealized_pnl": {
						"currency": "usd",
						"amount": "150.25",
						"decimals": 2,
						"display_decimals": 2
					},
					"hyperliquid": {
						"leverage": {
							"type": "cross",
							"value": 10
						},
						"liquidation_price": {
							"currency": "usd",
							"amount": "4912.40",
							"decimals": 2,
							"display_decimals": 2
						},
						"margin_used": {
							"currency": "usd",
							"amount": "435.00",
							"decimals": 2,
							"display_decimals": 2
						},
						"return_on_equity": "0.33",
						"cumulative_funding": {
							"all_time": {
								"currency": "usd",
								"amount": "1.20",
								"decimals": 2,
								"display_decimals": 2
							},
							"since_open": {
								"currency": "usd",
								"amount": "0.40",
								"decimals": 2,
								"display_decimals": 2
							},
							"since_change": {
								"currency": "usd",
								"amount": "0.10",
								"decimals": 2,
								"display_decimals": 2
							}
						}
					}
				}
			]
		},
		"spot": {
			"holdings": [
				{
					"market": "BTC",
					"average_entry_price": {
						"currency": "usd",
						"amount": "79841.12345678",
						"decimals": 8,
						"display_decimals": 8
					},
					"cost_basis": {
						"currency": "usd",
						"amount": "400.00",
						"decimals": 2,
						"display_decimals": 2
					},
					"unrealized_pnl": {
						"currency": "usd",
						"amount": "38.25",
						"decimals": 2,
						"display_decimals": 2
					},
					"unrealized_pnl_percentage": 9.5625,
					"realized_pnl": {
						"currency": "usd",
						"amount": "3.25",
						"decimals": 2,
						"display_decimals": 2
					}
				}
			]
		}
	},
	"balances": [
		{
			"balance": "1250.5",
			"breakdown": {
				"available": "1200.50",
				"in_transit": "0",
				"perps": "0",
				"pending": "50.00",
				"reserve": "0",
				"pending_settlements": [
					{
						"amount": "50.00",
						"date": "2026-08-01"
					}
				],
				"finishing_settlement": "0"
			},
			"icon_url": "https://cdn.whop.com/tokens/usdt.png",
			"name": "Tether USD",
			"price_usd": 1,
			"symbol": "USDT",
			"value_usd": "1250.50"
		},
		{
			"balance": "45.00",
			"breakdown": {
				"available": "40.00",
				"in_transit": "0",
				"perps": "0",
				"pending": "5.00",
				"reserve": "0",
				"pending_settlements": [
					{
						"amount": "3.00",
						"date": "2026-08-01"
					},
					{
						"amount": "2.00",
						"date": "2026-08-04"
					}
				],
				"finishing_settlement": "0"
			},
			"icon_url": null,
			"name": "EUR",
			"price_usd": 1.11,
			"symbol": "EUR",
			"value_usd": "50.00"
		}
	],
	"platform_credits": [
		{
			"amount": "25.00",
			"currency": "usd",
			"decimals": 2,
			"display_decimals": 2
		}
	],
	"banner_image_url": "https://cdn.whop.com/banner.png",
	"business_type": "services",
	"can_transfer_pending_balance_to_children": false,
	"cancellation_policy": null,
	"country": "US",
	"created_at": "2026-06-01T12:00:00Z",
	"description": "Mobile ceramic coating, paint correction, and interior detailing across the Austin metro.",
	"economic_intelligence": false,
	"email": "marcus@shinetime.example",
	"eula": null,
	"external_id": null,
	"home_preferences": ["hide_member_count"],
	"industry_type": "car_detailing",
	"invoice_prefix": "SHINE",
	"company_formation": {
		"status": "filed",
		"legal_name": "Shine Time Auto Detailing, LLC",
		"state_registered": true,
		"ein_registered": false,
		"signatures": {
			"ss4": {
				"status": "pending",
				"url": "https://esign.doola.com/session/sess_1a2b3c",
				"expires_at": "2026-08-01T00:00:00Z"
			}
		},
		"documents": [
			{
				"id": "file_marcuswebb123",
				"name": "Articles of Organization",
				"type": "articles_of_organization",
				"url": "https://img.whop.com/documents/articles-of-organization.pdf"
			}
		]
	},
	"logo_url": "https://cdn.whop.com/logo.png",
	"metadata": {
		"external_merchant_id": "merchant_123"
	},
	"onboarding_type": "seller",
	"opengraph_image_url": "https://cdn.whop.com/og.png",
	"opengraph_image_variant": "black",
	"other_business_description": "Mobile auto detailing",
	"other_industry_description": "Automotive services",
	"owner": {
		"id": "user_xxxxxxxxxxxx",
		"username": "marcuswebb",
		"name": "Marcus Webb",
		"profile_picture": {
			"url": "https://cdn.whop.com/user.png"
		},
		"badges": [
			{
				"type": "verified_partner"
			}
		]
	},
	"parent_account": {
		"id": "biz_platform123",
		"title": "Shine Time Partners",
		"route": "shine-time-partners",
		"logo_url": "https://cdn.whop.com/shine-time-partners-logo.png"
	},
	"privacy_policy": null,
	"require_2fa": true,
	"dispute_fighter_enabled": false,
	"orchestration_enabled": true,
	"collect_vat_id": false,
	"capabilities": {
		"accept_card_payments": "active",
		"accept_bank_payments": "active",
		"accept_bnpl_payments": "inactive",
		"standard_payout": "inactive",
		"instant_payout": "inactive",
		"crypto_payout": "inactive",
		"transfer": "inactive",
		"bank_deposit": "active",
		"crypto_deposit": "active",
		"card_deposit": "active",
		"card_issuing": "inactive",
		"run_ads": "active"
	},
	"payment_controls": {
		"dispute_alert_auto_refund": {
			"threshold_usd": 250,
			"locked": false
		},
		"resolution_center_auto_refund": {
			"card_threshold_usd": null,
			"financing_threshold_usd": null,
			"paypal_threshold_usd": null,
			"message": null,
			"locked": false
		},
		"dispute_alert_fee_usd": 29,
		"high_risk_processing_fee_percentage": 0,
		"pending_auto_topup_fee_percentage": 2,
		"pending_balance_delay_days": 0,
		"financing_disabled": false,
		"enforce_3ds": false,
		"restricted_payment_methods": [],
		"payment_method_warnings": [],
		"reserve": {
			"percentage": null,
			"hold_period_days": 90
		},
		"withdrawal_schedule": {
			"frequency": "monthly",
			"day": 1,
			"next_payout_date": "2026-09-01"
		},
		"undated_pending_reason": null
	},
	"cards": {
		"kind": "business",
		"status": "approved",
		"decline_reason": null
	},
	"financing": null,
	"required_actions": [
		{
			"action": "verify_identity",
			"status": "required",
			"title": "Identity verification required",
			"description": "Complete verification before your total earnings exceed $5k to continue accepting payments.",
			"cta_label": "Verify now",
			"cta": "https://whop.com/dashboard/biz_xxxxxxxxxxxxx/balance/?verify=true",
			"icon_url": null,
			"blocked_capabilities": [
				"accept_card_payments",
				"accept_bank_payments",
				"standard_payout",
				"instant_payout",
				"crypto_payout",
				"transfer",
				"bank_deposit",
				"card_issuing"
			]
		}
	],
	"recommended_actions": [
		{
			"action": "migrate_from_stripe",
			"status": "optional",
			"title": "Migrate your business from Stripe.",
			"description": "",
			"cta_label": "Get started",
			"cta": "https://whop.com/dashboard/biz_xxxxxxxxxxxxx/settings/stripe-migrations/",
			"icon_url": "https://whop.com/illustrations/orange/crane.svg",
			"blocked_capabilities": [],
			"reasoning": null,
			"impact_score": null
		},
		{
			"action": "accept_first_payment",
			"status": "optional",
			"title": "Accept your first payment.",
			"description": "",
			"cta_label": "Create payment link",
			"cta": "https://whop.com/dashboard/biz_xxxxxxxxxxxxx/links/checkout/create/",
			"icon_url": "https://whop.com/illustrations/orange/card.svg",
			"blocked_capabilities": [],
			"reasoning": null,
			"impact_score": null
		},
		{
			"action": "apply_for_financing",
			"status": "optional",
			"title": "Offer financing at checkout",
			"description": "Let customers pay over time with buy now, pay later.",
			"cta_label": "Apply",
			"cta": "https://whop.com/dashboard/biz_xxxxxxxxxxxxx/settings/payments/",
			"icon_url": "https://whop.com/illustrations/orange/piggy-bank.svg",
			"blocked_capabilities": ["accept_bnpl_payments"],
			"reasoning": null,
			"impact_score": null
		}
	],
	"return_policy": null,
	"route": "shine-time-auto-detailing",
	"shipping_policy": null,
	"website": "https://shinetime.example",
	"send_customer_emails": true,
	"payment_method_configuration": {
		"enabled": ["klarna"],
		"disabled": ["crypto"],
		"include_platform_defaults": true
	},
	"show_joined_whops": false,
	"show_reviews_dtc": true,
	"show_user_directory": false,
	"status": "active",
	"status_reason": null,
	"funds_release_at": null,
	"terms_of_service": null,
	"three_ds_level": "mandate_challenge",
	"statement_descriptor": "WHOP*SHINETIME",
	"merchant_of_record": "whop",
	"total_earned_usd": 3450,
	"ads_spend_usd": 180.25,
	"volume_usd": 4250.5,
	"volume_l30d_usd": 1200,
	"customers_l30d": 24,
	"level": {
		"current": 1,
		"points": 41500,
		"points_by_source": {
			"cards": 40000,
			"ads": 0,
			"trading": 1000,
			"partners": 0,
			"actions": 500
		},
		"resets_at": "2026-11-01T04:00:00.000Z",
		"account_level": 0
	},
	"social_links": [
		{
			"id": "social_marcuswebb123",
			"title": "Shine Time Auto Detailing",
			"url": "https://shinetime.example",
			"website": "website"
		}
	],
	"store_page_config": {
		"accent_color": "red",
		"layout": "compact",
		"profile_variant": "business",
		"whop_affiliate_link": true
	},
	"target_audience": "Owners of new and enthusiast vehicles in Austin, TX",
	"tax_collection_enabled_states": ["CA", "NY"],
	"tax_remitted_by": "whop",
	"tax_type": "exclusive",
	"product_tax_code": {
		"id": "ptc_CzLNn2Z058xEC1",
		"name": "Services",
		"product_type": "services"
	},
	"business_address": {
		"line1": "4180 Burnet Rd",
		"line2": null,
		"city": "Austin",
		"state": "TX",
		"postal_code": "78701",
		"country": "US"
	},
	"business_name": "Shine Time Auto Detailing, LLC",
	"tax_identifiers": [
		{
			"id": "taxid_marcuswebb123",
			"tax_id_type": "us_ein",
			"tax_id_value": "12-3456789"
		}
	],
	"title": "Shine Time Auto Detailing",
	"total_usd": "1300.50",
	"use_logo_as_opengraph_image_fallback": true,
	"verification": {
		"business": null,
		"individual": {
			"status": "approved"
		}
	},
	"wallet": {
		"id": "wallet_marcuswebb123",
		"address": "So11111111111111111111111111111111111111112",
		"network": "solana"
	}
}
```

## Business types and industries

Three fields classify accounts: `business_type`, `industry_group`, and `industry_type`. The full taxonomy lives on its own page:

> [card] [Business types and industries](https://docs.whop.com/api-reference/accounts/business-types.md)


## Related topics

- [External Accounts API](https://docs.whop.com/api-reference/external-accounts/external-account.md)
- [Social Accounts API (Deprecated)](https://docs.whop.com/api-reference/social-accounts/social-account.md)
- [List Accounts](https://docs.whop.com/api-reference/accounts/list-accounts.md)
