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

# Ads API Overview

> Launch Whop Ads through the API by generating a creative, then creating the campaign, ad group, and ad in one request.

Whop Ads runs paid ads on external ad networks directly from your Whop account. Ads spend is billed to the account's configured card or balance across its campaigns. Whop handles the ad account, review, launch, and billing.

Two API calls take you from nothing to a live ad: generate a creative, then create the campaign, ad group, and ad in one request.

## Resources

| Resource | Purpose |
| - | - |
| [Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign) | Top-level container: objective and (optionally) budget. |
| [Ad Group](https://docs.whop.com/api-reference/ad-groups/ad-group) | Targeting and budget for a set of ads. |
| [Ad](https://docs.whop.com/api-reference/ads/ad) | The creative unit — copy, assets, destination URL. |
| [Audience](https://docs.whop.com/api-reference/audiences/audience) | A custom audience to target or exclude in ad groups. |

## Billing and payment recovery

Spend is collected at account level across campaigns. A failed payment blocks delivery with `delivery_status: "payment_failed"` while preserving each campaign's configured active/paused `status`. Fix the account's payment method, then call [Retry Failed Ads Payments](https://docs.whop.com/api-reference/accounts/retry-failed-ads-payments.md) once for the account. The response queues a background attempt; it does not confirm payment. After settlement, active campaigns can resume and paused campaigns stay paused, subject to other delivery requirements.

See [Paying for ads](https://docs.whop.com/developer/ads/overview.md#paying-for-ads) for the dashboard flow, request example, duplicate-request behavior, and legacy campaign handling.

## One-time setup

* **Scopes**: `media:generate`, `media:read`, `ad_campaign:create`, `ad_campaign:update`, `ad_campaign:basic:read`.
* **Ads payment method**: connected in the dashboard (balance or card). Only launching needs it — drafts work without.
* **Logo and banner image**: the account needs both to launch. Set `logo` and `banner_image` with [`PATCH /accounts/{id}`](https://docs.whop.com/api-reference/accounts/update-account), or in the dashboard.

**1. Generate a creative**

```bash theme={null}
curl -X POST "https://api.whop.com/api/v1/media/generate" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"type": "image", "prompt": "A running club jogging across a bridge at sunrise, warm light, joyful energy"}'
```

Billed from your balance (`amount_charged`; not enough balance → `402` with a `deposit_url`). Poll [`GET /media/{id}`](https://docs.whop.com/api-reference/media/retrieve-media-asset) until `status` is `ready`, then use `file.id`:

```json theme={null}
{
	"id": "media_Hx7qg4tZc97o",
	"status": "ready",
	"file": { "id": "file_Zsv4DYkrbWE7h" }
}
```

`failed` refunds the charge — tweak the prompt and retry. Video: `"type": "video"` plus optional `duration_seconds` (5/10/15), `resolution` (`480p`–`4k`), and up to 4 `reference_media` file IDs (first one seeds the opening frame).

**2. Create and launch the ad — one request**

[`POST /ads`](https://docs.whop.com/api-reference/ads/create-an-ad) with an inline `ad_group` and `ad_campaign` creates the whole tree in one transaction and launches it:

```bash theme={null}
curl -X POST "https://api.whop.com/api/v1/ads" \
  -H "Authorization: Bearer $WHOP_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Run club launch ad",
    "primary_texts": ["Join 10,000 runners chasing their next PR together."],
    "headlines": ["Find your stride"],
    "call_to_action": "sign_up",
    "url": "https://whop.com/your-store-page",
    "creatives": [{"id": "file_Zsv4DYkrbWE7h"}],
    "ad_group": {
      "title": "US broad",
      "budget_amount": 25,
      "budget_type": "daily",
      "desired_cost_per_result": 15,
      "regions": {"include": {"countries": ["US"]}},
      "ad_campaign": {"title": "Run club growth", "objective": "sales"}
    }
  }'
```

Not ready to spend? Add `"status": "draft"` to `ad_campaign`, then launch later with [`PATCH /ad_campaigns/{id}`](https://docs.whop.com/api-reference/ad-campaigns/update-an-ad-campaign) `{"status": "active"}`.

| Field | Rule |
| - | - |
| `creatives` | One entry with no `format` is required (the base asset). `square` / `vertical` / `horizontal` are optional crops on top. |
| `desired_cost_per_result` | Required to launch. The cost per result you want to pay, in USD. Set it on each ad group. |
| `url` | Required for website ads. Your whop.com store page works as-is. An external page needs your [Whop pixel](https://docs.whop.com/developer/ads/pixel.md) installed. |
| Budget | One level owns it: `budget_amount` on the ad group (default), or on the campaign with `budget_optimization: "ad_campaign"`, never both. |
| Currency | `budget_amount` is USD, which budgets are stored and billed in. To state a budget in the account's `ads_reporting_currency` instead, send `budget_amount_local`. Whop converts it at the current rate and stores the USD result. Send one or the other, never both. Responses carry `budget_amount`, `budget_amount_local`, and `budget_currency`. |
| Targeting | Omit `demographics` / `devices` for automatic optimization. `regions` uses ISO 3166 (`"US"`, `"US-CA"`). |

To reuse existing containers, pass `ad_group_id` instead of `ad_group`, or `ad_campaign_id` instead of `ad_campaign`.

**3. Monitor**

Poll [`GET /ads/{id}`](https://docs.whop.com/api-reference/ads/retrieve-an-ad): `delivery_status` is the live state (a brief `in_review` for content moderation is normal), and `issues[]` carries a human-readable `message` for anything the ad network rejected asynchronously.

Control delivery with [`pause`](https://docs.whop.com/api-reference/ads/pause-an-ad) / [`unpause`](https://docs.whop.com/api-reference/ads/unpause-an-ad) on ads, ad groups, or campaigns. Edit copy or creatives with [`PATCH /ads/{id}`](https://docs.whop.com/api-reference/ads/update-an-ad) — sending `creatives` replaces the whole set, so include the base entry.

## Errors

Every gate is a `400` whose message says what to fix:

| Error contains | Fix |
| - | - |
| `The Whop pixel was not detected on <url>` | Use your own whop.com store page, or [install the pixel](https://docs.whop.com/developer/ads/pixel.md) on the landing page. |
| `Connect an ads payment method before launching` | Create with `ad_campaign.status: "draft"`, have the user connect a payment method in the dashboard, then `PATCH` to `active`. |
| `Include a base creative` | Add a `creatives` entry with no `format`. |
| `A destination URL is required` | Pass `url`. |
| `Add a desired cost per result to these ad groups` | Set `desired_cost_per_result` on each ad group that the message names. |
| `to your business before launching a campaign` | Add the missing `logo` or `banner_image` with [`PATCH /accounts/{id}`](https://docs.whop.com/api-reference/accounts/update-account). Then launch again. |
| `budget_amount is required` / `can't be set` | Move the budget to the right level (see the earlier field rules). |
| `Provide either budget_amount (USD) or budget_amount_local` | The two fields named different budgets. Send one. An echoed `GET` body, whose local amount restates the stored one, is fine. |
| `503` `Exchange rate for <currency> is unavailable` | Whop has no fresh rate for the account's reporting currency right now. Retry in a few minutes. |
| `402` + `deposit_url` (media) | Send the user to `deposit_url` to top up, then retry. |


## Related topics

- [Advertise with Whop Ads](https://docs.whop.com/developer/ads/overview.md)
- [Whop API Overview](https://docs.whop.com/api-reference/overview.md)
- [Ads API](https://docs.whop.com/api-reference/ads/ad.md)
