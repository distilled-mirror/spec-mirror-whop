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

# Ad Campaigns API

> An Ad Campaign is the top-level container for paid ads. It sets the objective and budget strategy shared by its ad groups and ads.

An Ad Campaign is the top-level container for paid ads. It sets the objective and budget strategy shared by its [ad groups](https://docs.whop.com/api-reference/ad-groups/ad-group) and ads.

Use the Ad Campaigns API to create campaigns, list campaigns for an account, retrieve or update campaign settings, and pause or resume campaign delivery.

Ads billing combines eligible spend across the account's campaigns. A failed payment blocks delivery with `delivery_status: payment_failed` while preserving the configured active/paused `status`. Fix the account's payment method and [retry its ads payment](https://docs.whop.com/api-reference/accounts/retry-failed-ads-payments.md) once for the account. The retry is asynchronous: acceptance does not confirm payment. Successful settlement clears the block; active campaigns can resume if otherwise eligible, while paused campaigns stay paused. See [billing and retries](https://docs.whop.com/developer/ads/overview.md#paying-for-ads).

## Endpoints

| Endpoint | Request |
| - | - |
| [List Ad Campaigns](https://docs.whop.com/api-reference/ad-campaigns/list-ad-campaigns.md) | GET `/ad_campaigns` |
| [Retrieve an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/retrieve-an-ad-campaign.md) | GET `/ad_campaigns/{id}` |
| [Create an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/create-an-ad-campaign.md) | POST `/ad_campaigns` |
| [Duplicate an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/duplicate-an-ad-campaign.md) | POST `/ad_campaigns/{id}/duplicate` |
| [Pause an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/pause-an-ad-campaign.md) | POST `/ad_campaigns/{id}/pause` |
| [Unpause an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/unpause-an-ad-campaign.md) | POST `/ad_campaigns/{id}/unpause` |
| [Update an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/update-an-ad-campaign.md) | PATCH `/ad_campaigns/{id}` |
| [Delete an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/delete-an-ad-campaign.md) | DELETE `/ad_campaigns/{id}` |

## Attributes

**id** `string` *required*

Unique identifier for the ad campaign, prefixed `adcamp_`.

**account** `object | null` *required*

The business running this campaign, with the name and logo its ads show.

> [accordion] Properties
>
> **id** `string` *required*
>
> Account ID, prefixed `biz_`.
>
> **banner\_image\_url** `string | null` *required*
>
> Account banner image URL. `null` when the account has not set one.
>
> **logo\_url** `string | null` *required*
>
> Account logo image URL. `null` when the account has not set one.
>
> **route** `string` *required*
>
> Account public route identifier, used in the `whop.com/\{route}` storefront
> path.
>
> **title** `string` *required*
>
> Account display name.

**added\_to\_cart\_value** `number` *required*

USD value attributed to add-to-cart events. Sums the value sent with each
event, normalized to USD; events without a value contribute 0.

**added\_to\_carts** `number` *required*

Whop pixel-attributed add-to-cart events, last-click.

**budget\_amount** `number | null` *required*

The campaign's budget in USD, which is what it is stored and billed in. `null`
when each ad group sets its own budget instead.

**budget\_amount\_local** `number | null` *required*

The same budget stated in `budget_currency` at today's exchange rate, for
display in the account's ads reporting currency. `null` when `budget_amount`
is.

**budget\_currency** `string` *required*

The ISO 4217 code `budget_amount_local` is in: the account's
`ads_reporting_currency` preference. `usd` unless the account changed it.

**budget\_optimization** `string | null` *required*

Which level owns the budget: the whole campaign (`ad_campaign`) or each ad group individually (`ad_group`).

Available options: `ad_campaign`, `ad_group`

**budget\_type** `string | null` *required*

Whether `budget_amount` is spent per day (`daily`) or over the campaign's full run (`lifetime`).

Available options: `daily`, `lifetime`

**campaign\_type** `string` *required*

The kind of campaign, fixed at creation. `standard` runs image and video ads; `search` shows text ads on search results for the ad groups' keywords.

Available options: `standard`, `search`

**click\_through\_rate** `number` *required*

Clicks divided by impressions, between 0 and 1.

**clicks** `number` *required*

The number of clicks.

**completed\_registration\_value** `number` *required*

USD value attributed to complete-registration events. Sums the value sent with
each event, normalized to USD; events without a value contribute 0.

**completed\_registrations** `number` *required*

Whop pixel-attributed complete-registration events, last-click.

**contact\_value** `number` *required*

USD value attributed to contact events. Sums the value sent with each event,
normalized to USD; events without a value contribute 0.

**contacts** `number` *required*

Whop pixel-attributed contact events, last-click.

**cost\_per\_added\_to\_cart** `number | null` *required*

Spend divided by attributed add-to-cart events; null when they are not the
goal and none are attributed.

**cost\_per\_click** `number` *required*

Spend divided by clicks; 0 when there are no clicks.

**cost\_per\_completed\_registration** `number | null` *required*

Spend divided by attributed complete-registration events; null when they are
not the goal and none are attributed.

**cost\_per\_contact** `number | null` *required*

Spend divided by attributed contact events; null when contacts are not the
goal and none are attributed.

**cost\_per\_lead** `number | null` *required*

Spend divided by attributed leads; null when leads are not a goal and none are
attributed.

**cost\_per\_mille** `number` *required*

Spend per 1,000 impressions; 0 when there are no impressions.

**cost\_per\_purchase** `number | null` *required*

Spend divided by attributed purchases; null when purchases are not a goal and
none are attributed.

**cost\_per\_result** `number | null` *required*

Spend divided by Whop pixel-attributed results. For `impressions`, it's the
spend per 1,000 impressions. `null` when nothing Whop-attributable is being
optimized for.

**cost\_per\_schedule** `number | null` *required*

Spend divided by attributed schedule events; null when schedules are not the
goal and none are attributed.

**cost\_per\_submitted\_application** `number | null` *required*

Spend divided by attributed submit-application events; null when they are not
the goal and none are attributed.

**cost\_per\_unique\_click** `number | null` *required*

Spend divided by unique clicks; null when there are no unique clicks.

**cost\_per\_viewed\_content** `number | null` *required*

Spend divided by attributed view-content events; null when they are not the
goal and none are attributed.

**created\_at** `string` *required*

When the campaign was created, as an ISO 8601 timestamp.

**custom\_conversions** `number` *required*

Whop pixel-attributed custom (merchant-defined) conversion events, last-click,
across all custom event names.

**custom\_event\_counts** `object` *required*

Whop pixel-attributed custom conversions, keyed by your event name with its
last-click count as the value. Empty when no named custom events are
attributed. Custom events fired without a name are counted in
custom\_conversions but omitted here, so these values sum to at most
custom\_conversions.

**custom\_event\_values** `object` *required*

Conversion value attributed to each custom event, keyed by event name like
custom\_event\_counts. Sums the value passed to whop.track, normalized to USD;
events fired without a value contribute 0.

**delivery\_status** `string` *required*

Whether the campaign's ads are delivering right now, and if not, why. Content review is in\_review; a campaign waiting to start is processing. Account billing failures set payment\_failed without changing the configured status. Successful payment retry clears that block and recalculates delivery. When several states apply at once, the highest-precedence one is returned.

Available options: `payment_failed`, `rejected`, `draft`, `paused`, `in_review`, `learning`, `processing`, `revise_content`, `issues`, `scheduled`, `completed`, `active`

**desired\_cost\_per\_result** `number | null`

The cost per result, in USD, the advertiser wants this campaign to achieve. A
target only: it is not a bid, cost cap, or bid cap, and doesn't change how the
campaign's ads bid. An ad group's own `desired_cost_per_result` overrides it.
For an `awareness` campaign, it's the cost per 1,000 impressions. `null` when
no target is set.

**ends\_at** `string | null` *required*

When the campaign stops delivering, as an ISO 8601 timestamp. `null` when it
runs until paused, or when its ad groups set their own schedule.

**frequency** `number | null` *required*

Impressions divided by reach.

**impressions** `number` *required*

The number of impressions.

**issues** `object[]` *required*

Open issues affecting the campaign and its descendant ad groups and ads.

> [accordion] Properties
>
> **id** `string` *required*
>
> Unique identifier for the issue.
>
> **category** `string` *required*
>
> The kind of issue: information about delivery, a warning, or an error requiring attention.
>
> Available options: `information`, `warning`, `error`
>
> **message** `string` *required*
>
> A description of what the issue is and how it can be resolved.
>
> **resource\_id** `string | null` *required*
>
> The ID of the campaign, ad group, or ad the issue is attached to.
>
> **resource\_type** `string` *required*
>
> The type of resource the issue is attached to.
>
> Available options: `ad_campaign`, `ad_group`, `ad`
>
> **title** `string` *required*
>
> A short, creator-facing title for the issue: the rule a content-review rejection names, or a generic label.

**lead\_value** `number` *required*

USD value attributed to lead events. Sums the value sent with each event,
normalized to USD; events without a value contribute 0.

**leads** `number` *required*

Whop pixel-attributed leads, last-click.

**link\_clicks** `number` *required*

Page views on your destination from people who clicked the ad, recorded by the
Whop pixel. Counted the same way on every ad platform.

**objective** `string | null` *required*

The goal the campaign optimizes toward. `awareness` optimizes for impressions. `engagement` appears only on campaigns created before it was retired.

Available options: `awareness`, `traffic`, `engagement`, `leads`, `sales`

**purchase\_value** `number` *required*

USD value of pixel-attributed purchases.

**purchases** `number` *required*

Whop pixel-attributed purchases, last-click.

**reach** `number` *required*

The number of unique people who saw this.

**result\_event** `string | null` *required*

The Whop pixel event whose attributed count represents results. It's the optimization goal, or the highest-volume attributed event for a campaign that budgets per ad group. `impressions` counts reported impressions. `null` when the goal isn't a Whop-attributed event.

Available options: `purchase`, `lead`, `schedule`, `submit_application`, `contact`, `complete_registration`, `view_content`, `add_to_cart`, `custom`, `messaging_conversation`, `impressions`

**result\_event\_name** `string | null` *required*

The merchant-defined event name when result\_event is custom; null for the
standard events.

**results** `number | null` *required*

The Whop pixel-attributed count behind result\_event. When a campaign's ad
groups optimize different goals there is no single result\_event (it is null),
and this is instead the sum of each ad group's own attributed results. Null
when nothing Whop-attributable is being optimized for.

**return\_on\_ad\_spend** `number` *required*

Purchase value divided by spend, both in USD (a currency-neutral ratio); 0
when there is no spend.

**schedule\_value** `number` *required*

USD value attributed to schedule events. Sums the value sent with each event,
normalized to USD; events without a value contribute 0.

**schedules** `number` *required*

Whop pixel-attributed schedule events, last-click.

**special\_ad\_categories** `string[]` *required*

Regulated categories the campaign is declared under. Ads in these categories are subject to extra targeting restrictions. Empty when none apply.

Available options: `housing`, `employment`, `financial_products`, `politics`

**spend** `number` *required*

The amount charged, in spend\_currency.

**spend\_currency** `string | null` *required*

The ISO 4217 currency code of all monetary metrics.

**starts\_at** `string | null` *required*

When the campaign starts delivering, as an ISO 8601 timestamp. `null` when it
starts as soon as it launches, or when its ad groups set their own schedule.

**status** `string` *required*

The configured lifecycle status of the ad campaign. Billing failures preserve active or paused here and set delivery\_status to payment\_failed.

Available options: `active`, `paused`, `inactive`, `payment_failed`, `draft`, `duplicating`

**submitted\_application\_value** `number` *required*

USD value attributed to submit-application events. Sums the value sent with
each event, normalized to USD; events without a value contribute 0.

**submitted\_applications** `number` *required*

Whop pixel-attributed submit-application events, last-click.

**target\_cost\_per\_result** `number | null` *required*

The desired cost per result for `target_result_event`, in USD. For
`impressions`, it's the cost per 1,000 impressions. `null` when none is set or
the ad groups set different ones.

**target\_result\_event** `string | null` *required*

The event `target_cost_per_result` is priced per, such as `purchase` or
`lead`. `null` when the campaign has no single goal.

**title** `string` *required*

Display name of the ad campaign.

**unique\_click\_through\_rate** `number | null` *required*

Unique clicks divided by impressions, between 0 and 1.

**unique\_clicks** `number` *required*

People who clicked, reported by the Whop pixel, counted once per person.

**updated\_at** `string` *required*

When the campaign was last updated, as an ISO 8601 timestamp.

**viewed\_content\_value** `number` *required*

USD value attributed to view-content events. Sums the value sent with each
event, normalized to USD; events without a value contribute 0.

**viewed\_contents** `number` *required*

Whop pixel-attributed view-content events, last-click.

```json AdCampaign theme={null}
{
	"id": "adcamp_xxxxxxxxxx",
	"account": {
		"id": "biz_xxxxxxxxxxxxx",
		"title": "Shine Time Auto Detailing",
		"route": "shine-time-auto-detailing",
		"logo_url": "https://img.whop.com/shine-time-auto-detailing-logo.png",
		"banner_image_url": "https://img.whop.com/shine-time-auto-detailing-banner.png"
	},
	"added_to_cart_value": 2100,
	"added_to_carts": 42,
	"budget_amount": 250,
	"budget_amount_local": 250,
	"budget_currency": "usd",
	"budget_optimization": "ad_campaign",
	"budget_type": "daily",
	"campaign_type": "standard",
	"click_through_rate": 0.032,
	"clicks": 1200,
	"completed_registration_value": 0,
	"completed_registrations": 28,
	"contact_value": 0,
	"contacts": 18,
	"cost_per_added_to_cart": 17.86,
	"cost_per_click": 0.63,
	"cost_per_completed_registration": 26.79,
	"cost_per_contact": 41.67,
	"cost_per_lead": 30,
	"cost_per_mille": 20,
	"cost_per_purchase": 50,
	"cost_per_result": 50,
	"cost_per_schedule": null,
	"cost_per_submitted_application": null,
	"cost_per_unique_click": 0.74,
	"cost_per_viewed_content": 1.25,
	"created_at": "2026-06-01T12:00:00Z",
	"custom_conversions": 10,
	"custom_event_counts": {
		"QualifiedCall": 7,
		"BookedDemo": 3
	},
	"custom_event_values": {
		"QualifiedCall": 3500,
		"BookedDemo": 0
	},
	"delivery_status": "issues",
	"ends_at": null,
	"frequency": 2.1,
	"impressions": 37500,
	"issues": [
		{
			"id": "adiss_xxxxxxxxxxx",
			"category": "error",
			"title": "Ad issue",
			"message": "The ad's creative violates the ad network's policies. Edit the creative and resubmit for review.",
			"resource_id": "adcamp_xxxxxxxxxx",
			"resource_type": "ad_campaign"
		}
	],
	"lead_value": 1250,
	"leads": 25,
	"link_clicks": 960,
	"objective": "sales",
	"purchase_value": 12600,
	"purchases": 15,
	"reach": 17800,
	"result_event": "purchase",
	"result_event_name": null,
	"results": 15,
	"return_on_ad_spend": 16.8,
	"schedule_value": 0,
	"schedules": 0,
	"special_ad_categories": [],
	"spend": 750,
	"spend_currency": "usd",
	"starts_at": null,
	"status": "active",
	"submitted_application_value": 0,
	"submitted_applications": 0,
	"target_cost_per_result": 12.5,
	"target_result_event": "purchase",
	"title": "Ceramic coating spring launch",
	"unique_click_through_rate": 0.027,
	"unique_clicks": 1010,
	"updated_at": "2026-06-02T12:00:00Z",
	"viewed_content_value": 0,
	"viewed_contents": 600
}
```


## Related topics

- [Ad Campaigns](https://docs.whop.com/api-reference/legacy/ad-campaigns/ad-campaign.md)
- [Delete an Ad Campaign](https://docs.whop.com/api-reference/ad-campaigns/delete-an-ad-campaign.md)
- [Ad campaign updated](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign-updated.md)
