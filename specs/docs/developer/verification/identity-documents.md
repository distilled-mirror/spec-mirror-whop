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

# Identity Documents

> Which identity documents the API accepts, and the exact file keys to send for each.

Verify a person entirely through the API by sending their identity document photos in the [Create Verification](https://docs.whop.com/api-reference/verifications/create-verification.md) request. No hosted session, no follow-up calls: one `POST`, then poll for the verdict.

## Which files to send

Pick the `document_type` that matches the document in the photograph. It decides exactly which keys the `documents` object must contain — nothing more, nothing less.

| `document_type` | Required `documents` keys |
| - | - |
| `id_card` | `id_card_front`, `id_card_back`, `selfie` |
| `drivers` | `drivers_front`, `drivers_back`, `selfie` |
| `residence_permit` | `residence_permit_front`, `residence_permit_back`, `selfie` |
| `passport` | `passport_front`, `selfie` |

Every value is the file's raw bytes, base64-encoded:

```python theme={null}
import base64

def b64(path):
    with open(path, "rb") as f:
        return base64.b64encode(f.read()).decode()
```

## File rules

* Joint Photographic Experts Group (JPEG), Portable Network Graphics (PNG), or PDF. Whop detects the type from the file's actual bytes, so the extension doesn't matter.
* The `selfie` must be a JPEG or PNG photo of the person's face.
* 5MB maximum per file, measured before encoding.
* Send the complete set in one request. A missing, oversized, or unreadable file fails the whole request with an error naming the exact key — fix that file and send the same request again. Nothing partial is ever submitted.

## Full example

```bash theme={null}
curl -X POST "https://api.whop.com/api/v1/verifications?account_id=biz_XXXXXXXX" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"first_name\": \"Jane\",
    \"last_name\": \"Doe\",
    \"date_of_birth\": \"1990-01-01\",
    \"country\": \"US\",
    \"phone\": \"+15551234567\",
    \"tax_identification_number\": \"123456789\",
    \"address\": { \"line1\": \"123 Main St\", \"city\": \"Austin\", \"state\": \"TX\", \"postal_code\": \"73301\" },
    \"business_website\": \"https://example.com\",
    \"document_type\": \"drivers\",
    \"documents\": {
      \"drivers_front\": \"$(base64 -i front.jpg)\",
      \"drivers_back\": \"$(base64 -i back.jpg)\",
      \"selfie\": \"$(base64 -i selfie.jpg)\"
    }
  }"
```

A `201` means Whop accepted every document and started the review — the response's `required_documents` lists each key as `submitted`. Poll [Retrieve Verification](https://docs.whop.com/api-reference/verifications/retrieve-verification.md) until `status` becomes `approved` or `rejected`.

> [!NOTE]
>
> `business_website` is optional. When omitted, the account's whop.com page is used for payout setup. If you send one, it must be an external site or social link. Whop store pages are rejected.

## If Whop rejects a document

When the reviewer can't accept a photo, `status` returns to `action_required` and the rejected keys appear in `required_documents` as `pending_upload` with a `rejection_reason`. Send the same Create Verification request again with a better photo — the review resumes automatically.

## Country coverage

Whop accepts `id_card`, `drivers`, and `passport` for people in most countries. The table below lists the exceptions. For a country in this table, only the listed values work. Anything else comes back through the previous rejection flow.

| Country | Accepted `document_type` values |
| - | - |
| `AD` | `drivers`, `residence_permit`, `passport` |
| `AI` | `drivers`, `passport` |
| `AQ` | `passport` |
| `AS` | `drivers`, `passport` |
| `CC` | `id_card`, `passport` |
| `CK` | `drivers`, `passport` |
| `CX` | `drivers`, `passport` |
| `EH` | `passport` |
| `ER` | `id_card`, `passport` |
| `FK` | `passport` |
| `FM` | `drivers`, `residence_permit`, `passport` |
| `FO` | `drivers`, `passport` |
| `GF` | `id_card`, `residence_permit`, `passport` |
| `GL` | `drivers`, `passport` |
| `HM` | `passport` |
| `KY` | `drivers`, `passport` |
| `MS` | `drivers`, `residence_permit`, `passport` |
| `NF` | `passport` |
| `PM` | `id_card`, `passport` |
| `SH` | `passport` |
| `SJ` | `passport` |
| `TF` | `passport` |
| `TK` | `drivers`, `passport` |
| `TV` | `drivers`, `passport` |
| `VA` | `passport` |
| `WF` | `id_card`, `passport` |
| `YT` | `id_card`, `passport` |

Whop accepts `residence_permit` in most countries too, but its coverage is narrower. Beyond the previous table, Whop **doesn't** accept it for people in:

`AG`, `AW`, `BB`, `BF`, `BI`, `BL`, `BZ`, `CV`, `CW`, `DM`, `GG`, `GI`, `GN`, `GS`, `GU`, `GY`, `IM`, `IO`, `JE`, `KI`, `KM`, `LC`, `LR`, `MF`, `ML`, `MP`, `MQ`, `MU`, `NC`, `NR`, `NU`, `NZ`, `PF`, `PG`, `PN`, `PR`, `PS`, `PW`, `SB`, `SC`, `SM`, `SR`, `ST`, `SX`, `SY`, `SZ`, `TC`, `TL`, `TO`, `VC`, `VI`, `WS`

Whop doesn't offer identity verification to people in these countries and declines every document type:

`AF` (Afghanistan), `BY` (Belarus), `CU` (Cuba), `HT` (Haiti), `IQ` (Iraq), `IR` (Iran), `KP` (North Korea), `LY` (Libya), `MM` (Myanmar), `MZ` (Mozambique), `RU` (Russia), `SD` (Sudan), `SO` (Somalia), `SS` (South Sudan)

> [!NOTE]
>
> This table reflects Whop's verification configuration as of July 2026. Country acceptance is applied during review, so a document type that isn't valid for the person's country fails with a `rejection_reason` rather than an upfront error.


## Related topics

- [Handle Verification RFIs](https://docs.whop.com/developer/verification/rfis.md)
- [Generate Verification Document URL](https://docs.whop.com/api-reference/verifications/generate-verification-document-url.md)
- [API Versions](https://docs.whop.com/developer/api/versioning.md)
