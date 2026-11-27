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

# Ads API

> An Ad is the individual creative unit delivered by an ad group. It holds the copy, creative assets, and destination URL for one ad.

An Ad is the individual creative unit delivered by an [ad group](https://docs.whop.com/api-reference/ad-groups/ad-group). It holds the copy, creative assets, and destination URL for one ad.

Use the Ads API to list ads for an account, create ads inside ad groups, retrieve or update creative details, delete ads that should stop running, and pause or resume delivery.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Ads](https://docs.whop.com/api-reference/ads/list-ads.md) | GET `/ads` |
| [Retrieve an Ad](https://docs.whop.com/api-reference/ads/retrieve-an-ad.md) | GET `/ads/{id}` |
| [Create an Ad](https://docs.whop.com/api-reference/ads/create-an-ad.md) | POST `/ads` |
| [Appeal an Ad](https://docs.whop.com/api-reference/ads/appeal-an-ad.md) | POST `/ads/{id}/appeal` |
| [Duplicate an Ad](https://docs.whop.com/api-reference/ads/duplicate-an-ad.md) | POST `/ads/{id}/duplicate` |
| [Pause an Ad](https://docs.whop.com/api-reference/ads/pause-an-ad.md) | POST `/ads/{id}/pause` |
| [Unpause an Ad](https://docs.whop.com/api-reference/ads/unpause-an-ad.md) | POST `/ads/{id}/unpause` |
| [Update an Ad](https://docs.whop.com/api-reference/ads/update-an-ad.md) | PATCH `/ads/{id}` |
| [Delete an Ad](https://docs.whop.com/api-reference/ads/delete-an-ad.md) | DELETE `/ads/{id}` |

## Attributes

**id** `string` *required*

Unique identifier for the ad, prefixed `ad_`.

**ad\_campaign** `object` *required*

The ad campaign this ad belongs to.

> [accordion] Properties
>
> **id** `string` *required*
>
> The referenced entity's id.

**ad\_group** `object` *required*

The ad group this ad belongs to.

> [accordion] Properties
>
> **id** `string` *required*
>
> The referenced entity's id.

**added\_to\_cart\_value** `number` *required*

USD value attributed to add-to-cart events. Sums the value sent with each
event, normalized to USD; events without a value contribute 0.

**added\_to\_carts** `number` *required*

Whop pixel-attributed add-to-cart events, last-click.

**appeal\_available\_at** `string | null` *required*

When the ad can next be appealed, as an ISO 8601 timestamp. `null` when it can
be appealed now.

**call\_to\_action** `string | null` *required*

The call-to-action button shown on the ad.

Available options: `learn_more`, `shop_now`, `sign_up`, `subscribe`, `get_started`, `book_now`, `apply_now`, `contact_us`, `download`, `order_now`, `buy_now`, `get_quote`, `message_page`, `whatsapp_message`, `instagram_message`, `call_now`, `get_directions`, `send_updates`, `get_offer`, `watch_more`, `listen_now`, `play_game`, `open_link`, `no_button`, `get_offer_view`, `get_event_tickets`, `see_menu`, `request_time`, `event_rsvp`, `see_details`, `view_instagram_profile`, `donate_now`, `see_more`, `visit_site`

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

**content\_review** `object` *required*

Whop's content review of this ad: whether it has finished and the content rules it breaks.

> [accordion] Properties
>
> **blocked\_by\_website** `boolean` *required*
>
> Whether the ad is held back for a rule the review found on its website.
> Changing the website and rechecking it can clear the ad.
>
> **status** `string` *required*
>
> `reviewed` once Whop content review finishes, `in_review` while it runs, `not_reviewed` when it never ran.
>
> Available options: `reviewed`, `in_review`, `not_reviewed`
>
> **tags** `object[]` *required*
>
> Every content rule the review found the ad breaks. Empty when it breaks none or the review hasn't finished.
>
> > [accordion] Properties
> >
> > **blocking** `boolean` *required*
> >
> > Whether this rule keeps the ad from running until it's fixed. `false` when the
> > ad can still run and fixing it only lets the ad run on more networks.
> >
> > **locations** `string[]` *required*
> >
> > Where on the ad the review found this. `creative` is the image or video, and `audio` is what is said or heard in it. `copy` is the ad's text, and `destination` is the landing page. Empty on reviews saved before findings recorded where they were found.
> >
> > Available options: `creative`, `audio`, `copy`, `destination`
> >
> > **message** `string` *required*
> >
> > The evidence the review recorded for this rule.
> >
> > **title** `string` *required*
> >
> > The rule Whop content review found the ad breaks.

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

When the ad was created, as an ISO 8601 timestamp.

**creatives** `object[]` *required*

The creative assets used by this ad. The original asset has a null format; square, vertical, and horizontal entries are placement-specific variants. A carousel ad returns one format-null entry per attachment, in order.

> [accordion] Properties
>
> **id** `string` *required*
>
> The creative attachment's file id.
>
> **crop** `object | null` *required*
>
> The saved crop window for this creative, in source image pixels. Null for the original asset or a format that has not been cropped.
>
> > [accordion] Properties
> >
> > **height** `number` *required*
> >
> > Height of the crop window in source pixels.
> >
> > **width** `number` *required*
> >
> > Width of the crop window in source pixels.
> >
> > **x** `number` *required*
> >
> > Left edge of the crop window in source pixels.
> >
> > **y** `number` *required*
> >
> > Top edge of the crop window in source pixels.
>
> **format** `string | null` *required*
>
> The placement variant this asset covers, or null for the original asset.
>
> Available options: `square`, `vertical`, `horizontal`
>
> **language** `string | null` *required*
>
> Always `null`. Translations are no longer available.
>
> **media\_type** `string | null` *required*
>
> The kind of asset, image or video.
>
> **source\_id** `string | null` *required*
>
> The file this format was cut out of, prefixed `file_`. Null for the original
> asset or a format uploaded as it is.
>
> **url** `string | null` *required*
>
> CDN url of the asset.

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

Whether the ad is delivering right now, and if not, why. When several states apply at once, the highest-precedence one is returned.

Available options: `rejected`, `in_review`, `draft`, `paused`, `processing`, `revise_content`, `issues`, `scheduled`, `learning`, `active`

**descriptions** `object[]` *required*

The description shown on the ad.

> [accordion] Properties
>
> **language** `string | null` *required*
>
> Always `null`. Translations are no longer available.
>
> **text** `string` *required*
>
> The text shown to viewers.

**frequency** `number | null` *required*

Impressions divided by reach.

**headlines** `object[]` *required*

The headline shown on the ad.

> [accordion] Properties
>
> **language** `string | null` *required*
>
> Always `null`. Translations are no longer available.
>
> **text** `string` *required*
>
> The text shown to viewers.

**impressions** `number` *required*

The number of impressions.

**issues** `object[]` *required*

Open issues affecting this ad. Empty when there are none.

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

**primary\_texts** `object[]` *required*

The primary text shown in the ad body. Several entries make text variations.

> [accordion] Properties
>
> **language** `string | null` *required*
>
> Always `null`. Translations are no longer available.
>
> **text** `string` *required*
>
> The text shown to viewers.

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

**spend** `number` *required*

The amount charged, in spend\_currency.

**spend\_currency** `string | null` *required*

The ISO 4217 currency code of all monetary metrics.

**status** `string` *required*

Whether the ad is enabled. `active` and `paused` are set by you; `in_review` and `rejected` come from ad review.

Available options: `active`, `paused`, `in_review`, `rejected`

**submitted\_application\_value** `number` *required*

USD value attributed to submit-application events. Sums the value sent with
each event, normalized to USD; events without a value contribute 0.

**submitted\_applications** `number` *required*

Whop pixel-attributed submit-application events, last-click.

**title** `string | null` *required*

Display title of the ad.

**unique\_click\_through\_rate** `number | null` *required*

Unique clicks divided by impressions, between 0 and 1.

**unique\_clicks** `number` *required*

People who clicked, reported by the Whop pixel, counted once per person.

**updated\_at** `string` *required*

When the ad was last updated, as an ISO 8601 timestamp.

**url** `string | null` *required*

The URL the ad links to, without its query string. Parameters belong in
`url_parameters`; any you send on `url` are moved there.

**url\_parameters** `object` *required*

Query parameters appended to `url`, keyed by name. Whop's click-attribution
parameters are reserved.

**viewed\_content\_value** `number` *required*

USD value attributed to view-content events. Sums the value sent with each
event, normalized to USD; events without a value contribute 0.

**viewed\_contents** `number` *required*

Whop pixel-attributed view-content events, last-click.

```json Ad theme={null}
{
	"id": "ad_xxxxxxxxxxxxx",
	"ad_campaign": {
		"id": "adcamp_xxxxxxxxxx"
	},
	"ad_group": {
		"id": "adgrp_xxxxxxxxxxx"
	},
	"added_to_cart_value": 540,
	"added_to_carts": 18,
	"call_to_action": "shop_now",
	"click_through_rate": 0.034,
	"clicks": 420,
	"completed_registration_value": 0,
	"completed_registrations": 12,
	"contact_value": 0,
	"contacts": 8,
	"content_review": {
		"status": "reviewed",
		"tags": [
			{
				"title": "Unrealistic earnings or investment claims",
				"message": "The ad promises viewers they will make $10,000 a month.",
				"blocking": false,
				"locations": ["copy"]
			}
		],
		"blocked_by_website": false
	},
	"cost_per_added_to_cart": 7.22,
	"cost_per_click": 0.31,
	"cost_per_completed_registration": 10.83,
	"cost_per_contact": 16.25,
	"cost_per_lead": 9.29,
	"cost_per_mille": 14.5,
	"cost_per_purchase": 21.67,
	"cost_per_result": 21.67,
	"cost_per_schedule": null,
	"cost_per_submitted_application": null,
	"cost_per_unique_click": 0.37,
	"cost_per_viewed_content": 0.54,
	"created_at": "2026-06-01T12:00:00Z",
	"creatives": [
		{
			"id": "file_xxxxxxxxxxxx",
			"crop": {
				"height": 1080,
				"width": 1080,
				"x": 120,
				"y": 0
			},
			"format": "square",
			"source_id": "file_yyyyyyyyyyyy",
			"media_type": "image",
			"url": "https://img.whop.com/file_xxxxxxxxxxxx.jpg",
			"language": null
		}
	],
	"custom_conversions": 4,
	"custom_event_counts": {
		"QualifiedCall": 3,
		"BookedDemo": 1
	},
	"custom_event_values": {
		"QualifiedCall": 1500,
		"BookedDemo": 0
	},
	"delivery_status": "issues",
	"descriptions": [
		{
			"language": null,
			"text": "Limited spots available this week."
		}
	],
	"frequency": 1.8,
	"headlines": [
		{
			"language": null,
			"text": "Join the Unlimited Wash Club"
		}
	],
	"impressions": 9000,
	"issues": [
		{
			"id": "adiss_xxxxxxxxxxx",
			"category": "error",
			"title": "Ad issue",
			"message": "The ad's creative violates the ad network's policies. Edit the creative and resubmit for review.",
			"resource_id": "ad_xxxxxxxxxxxxx",
			"resource_type": "ad"
		}
	],
	"lead_value": 700,
	"leads": 14,
	"link_clicks": 336,
	"primary_texts": [
		{
			"language": null,
			"text": "Build sharper trading habits with daily lessons."
		}
	],
	"purchase_value": 1820,
	"purchases": 6,
	"reach": 5000,
	"result_event": "purchase",
	"result_event_name": null,
	"results": 6,
	"return_on_ad_spend": 14,
	"schedule_value": 0,
	"schedules": 0,
	"spend": 130,
	"spend_currency": "usd",
	"status": "active",
	"submitted_application_value": 0,
	"submitted_applications": 0,
	"title": "Ceramic coating spring launch ad",
	"unique_click_through_rate": 0.028,
	"unique_clicks": 350,
	"updated_at": "2026-06-02T12:00:00Z",
	"url": "https://whop.com/shine-time-auto-detailing",
	"url_parameters": {
		"utm_campaign": "ceramic-coating-launch"
	},
	"viewed_content_value": 0,
	"viewed_contents": 240,
	"appeal_available_at": null
}
```


## Related topics

- [Ads](https://docs.whop.com/api-reference/legacy/ads/ad.md)
- [Ads API Overview](https://docs.whop.com/api-reference/ads/overview.md)
- [Advertise with Whop Ads](https://docs.whop.com/developer/ads/overview.md)
