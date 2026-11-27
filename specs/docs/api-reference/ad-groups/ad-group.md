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

# Ad Groups API

> An Ad Group sits inside an ad campaign and controls delivery for ads. It sets the audience, schedule, and budget for its ads.

An Ad Group sits inside an [ad campaign](https://docs.whop.com/api-reference/ad-campaigns/ad-campaign) and controls delivery for [ads](https://docs.whop.com/api-reference/ads/ad). It sets the audience, schedule, and budget for its ads.

Use the Ad Groups API to create ad groups in campaigns, list or retrieve targeting and delivery settings, update budgets or targeting, delete groups that should stop running, and pause or resume delivery.

## Endpoints

| Endpoint | Request |
| - | - |
| [List Ad Groups](https://docs.whop.com/api-reference/ad-groups/list-ad-groups.md) | GET `/ad_groups` |
| [Retrieve an Ad Group](https://docs.whop.com/api-reference/ad-groups/retrieve-an-ad-group.md) | GET `/ad_groups/{id}` |
| [Create an Ad Group](https://docs.whop.com/api-reference/ad-groups/create-an-ad-group.md) | POST `/ad_groups` |
| [Duplicate an Ad Group](https://docs.whop.com/api-reference/ad-groups/duplicate-an-ad-group.md) | POST `/ad_groups/{id}/duplicate` |
| [Pause an Ad Group](https://docs.whop.com/api-reference/ad-groups/pause-an-ad-group.md) | POST `/ad_groups/{id}/pause` |
| [Unpause an Ad Group](https://docs.whop.com/api-reference/ad-groups/unpause-an-ad-group.md) | POST `/ad_groups/{id}/unpause` |
| [Update an Ad Group](https://docs.whop.com/api-reference/ad-groups/update-an-ad-group.md) | PATCH `/ad_groups/{id}` |
| [Delete an Ad Group](https://docs.whop.com/api-reference/ad-groups/delete-an-ad-group.md) | DELETE `/ad_groups/{id}` |

## Targeting restrictions

Ad groups cannot target the [countries listed in the advertising guide](https://docs.whop.com/developer/ads/overview.md#countries-you-cannot-target). Requests to create an ad group or update its targeted countries are rejected if they include a blocked country in `regions.include.countries`.

## Attributes

**id** `string` *required*

Unique identifier for the ad group, prefixed `adgrp_`.

**ad\_campaign** `object` *required*

The ad campaign this ad group belongs to.

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

**audiences** `object` *required*

Saved audiences this ad group delivers to or excludes.

> [accordion] Properties
>
> **exclude** `string[]` *required*
>
> IDs of saved audiences excluded from delivery, prefixed `adaud_`.
>
> **include** `string[]` *required*
>
> IDs of saved audiences the ad group delivers to, prefixed `adaud_`.

**bid\_type** `string | null` *required*

How delivery bids are set in the ad auction. Target-based strategies use `desired_cost_per_result`.

Available options: `minimum_cost`, `average_target`, `maximum_target`

**budget\_amount** `number | null` *required*

This ad group's budget in USD, which is what it is stored and billed in.
`null` when the budget is set on the campaign instead.

**budget\_amount\_local** `number | null` *required*

The same budget stated in `budget_currency` at today's exchange rate, for
display in the account's ads reporting currency. `null` when `budget_amount`
is.

**budget\_currency** `string` *required*

The ISO 4217 code `budget_amount_local` is in: the account's
`ads_reporting_currency` preference. `usd` unless the account changed it.

**budget\_type** `string | null` *required*

Whether `budget_amount` is spent per day (`daily`) or over the ad group's full run (`lifetime`). A `lifetime` ad group also needs `ends_at`, at least 24 hours after it starts.

Available options: `daily`, `lifetime`

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

**conversion\_event** `string | null` *required*

The pixel event optimized for. A standard event, or any custom pixel event
name.

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

When the ad group was created, as an ISO 8601 timestamp.

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

**delivery\_schedule** `object | null` *required*

Hours the ad group delivers in each week, keyed by day. Days it doesn't deliver are omitted. `null` when it delivers at every hour. A schedule needs a lifetime budget on the ad group, or on its campaign when the campaign holds the budget. Only ad groups and campaigns that already have one can use it.

> [accordion] Properties
>
> **friday** `object[]`
>
> Windows the ad group delivers in on Friday, in time order. Omitted when it doesn't deliver that day.
>
> > [accordion] Properties
> >
> > **end** `string` *required*
> >
> > When delivery stops, as a 24-hour `HH:00` time up to `24:00` for midnight.
> > Later than `start`; a window past midnight is split across two days.
> >
> > **start** `string` *required*
> >
> > When delivery starts, as a 24-hour `HH:00` time from `00:00` to `23:00`.
>
> **monday** `object[]`
>
> Windows the ad group delivers in on Monday, in time order. Omitted when it doesn't deliver that day.
>
> > [accordion] Properties
> >
> > **end** `string` *required*
> >
> > When delivery stops, as a 24-hour `HH:00` time up to `24:00` for midnight.
> > Later than `start`; a window past midnight is split across two days.
> >
> > **start** `string` *required*
> >
> > When delivery starts, as a 24-hour `HH:00` time from `00:00` to `23:00`.
>
> **saturday** `object[]`
>
> Windows the ad group delivers in on Saturday, in time order. Omitted when it doesn't deliver that day.
>
> > [accordion] Properties
> >
> > **end** `string` *required*
> >
> > When delivery stops, as a 24-hour `HH:00` time up to `24:00` for midnight.
> > Later than `start`; a window past midnight is split across two days.
> >
> > **start** `string` *required*
> >
> > When delivery starts, as a 24-hour `HH:00` time from `00:00` to `23:00`.
>
> **sunday** `object[]`
>
> Windows the ad group delivers in on Sunday, in time order. Omitted when it doesn't deliver that day.
>
> > [accordion] Properties
> >
> > **end** `string` *required*
> >
> > When delivery stops, as a 24-hour `HH:00` time up to `24:00` for midnight.
> > Later than `start`; a window past midnight is split across two days.
> >
> > **start** `string` *required*
> >
> > When delivery starts, as a 24-hour `HH:00` time from `00:00` to `23:00`.
>
> **thursday** `object[]`
>
> Windows the ad group delivers in on Thursday, in time order. Omitted when it doesn't deliver that day.
>
> > [accordion] Properties
> >
> > **end** `string` *required*
> >
> > When delivery stops, as a 24-hour `HH:00` time up to `24:00` for midnight.
> > Later than `start`; a window past midnight is split across two days.
> >
> > **start** `string` *required*
> >
> > When delivery starts, as a 24-hour `HH:00` time from `00:00` to `23:00`.
>
> **tuesday** `object[]`
>
> Windows the ad group delivers in on Tuesday, in time order. Omitted when it doesn't deliver that day.
>
> > [accordion] Properties
> >
> > **end** `string` *required*
> >
> > When delivery stops, as a 24-hour `HH:00` time up to `24:00` for midnight.
> > Later than `start`; a window past midnight is split across two days.
> >
> > **start** `string` *required*
> >
> > When delivery starts, as a 24-hour `HH:00` time from `00:00` to `23:00`.
>
> **wednesday** `object[]`
>
> Windows the ad group delivers in on Wednesday, in time order. Omitted when it doesn't deliver that day.
>
> > [accordion] Properties
> >
> > **end** `string` *required*
> >
> > When delivery stops, as a 24-hour `HH:00` time up to `24:00` for midnight.
> > Later than `start`; a window past midnight is split across two days.
> >
> > **start** `string` *required*
> >
> > When delivery starts, as a 24-hour `HH:00` time from `00:00` to `23:00`.

**delivery\_status** `string` *required*

Whether ads in this ad group are delivering right now, and if not, why. When several states apply at once, the highest-precedence one is returned.

Available options: `rejected`, `draft`, `paused`, `in_review`, `processing`, `revise_content`, `issues`, `scheduled`, `completed`, `learning`, `active`

**demographics** `object` *required*

Age, gender, and automatic-audience targeting.

> [accordion] Properties
>
> **automatic** `boolean` *required*
>
> Whether automatic audience targeting is on. When `true`, delivery can reach
> beyond the ages and genders you set, treating them as suggestions.
>
> **gender** `string` *required*
>
> Gender targeted.
>
> Available options: `all`, `male`, `female`
>
> **maximum\_age** `number | null` *required*
>
> Oldest age targeted. `null` when no maximum is set.
>
> **minimum\_age** `number | null` *required*
>
> Youngest age targeted. `null` when no minimum is set.

**desired\_cost\_per\_result** `number | null` *required*

The cost per result the advertiser wants for this ad group, in USD. For an
`awareness` campaign, it's the cost per 1,000 impressions. `null` when the ad
group uses its campaign's `desired_cost_per_result`.

**devices** `object` *required*

Device platforms and operating systems targeted.

> [accordion] Properties
>
> **operating\_systems** `object[]` *required*
>
> Operating systems targeted. Empty targets all operating systems.
>
> > [accordion] Properties
> >
> > **minimum\_version** `string`
> >
> > Lowest OS version targeted, such as `18.0`. Absent when any version qualifies.
> >
> > **os** `string` *required*
> >
> > Operating system targeted.
> >
> > Available options: `ios`, `android`
>
> **platforms** `string[]` *required*
>
> Device types targeted. Empty targets all devices.
>
> Available options: `mobile`, `desktop`

**ends\_at** `string | null` *required*

When the ad group stops delivering, as an ISO 8601 timestamp. `null` when it
runs until paused.

**frequency** `number | null` *required*

Impressions divided by reach.

**impressions** `number` *required*

The number of impressions.

**issues** `object[]` *required*

Open issues affecting this ad group and its ads. Empty when there are none.

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

**keywords** `object[]`

Search terms this ad group's ads can show for, and terms they never show for. Search campaigns only, where each ad group needs at least one before launch.

> [accordion] Properties
>
> **match\_type** `string` *required*
>
> How closely a search has to match: `broad` also reaches related searches, `phrase` needs the meaning of the phrase, and `exact` needs the same meaning as the term. Defaults to `broad`.
>
> Available options: `broad`, `phrase`, `exact`
>
> **negative** `boolean` *required*
>
> Whether this term keeps the ads from showing instead of making them eligible.
> Defaults to `false`.
>
> **text** `string` *required*
>
> The search term, up to 80 characters and 10 words.

**languages** `string[]` *required*

Languages targeted, as ISO 639 codes such as `en` or `es`. A region-specific
locale with no ISO code appears as a numeric locale key. Empty targets all
languages.

**lead\_value** `number` *required*

USD value attributed to lead events. Sums the value sent with each event,
normalized to USD; events without a value contribute 0.

**leads** `number` *required*

Whop pixel-attributed leads, last-click.

**learning\_progress** `object | null` *required*

Progress toward the conversions this ad group's learning phase needs. Null unless it is learning or learning limited and valid counts are available. Reaching the threshold does not determine delivery status.

> [accordion] Properties
>
> **conversion\_threshold** `integer` *required*
>
> The conversions this ad group's learning phase needs. It can change during
> delivery.
>
> **conversions** `integer` *required*
>
> Conversions counted since the last significant edit during this ad group's
> learning phase.
>
> **progress\_percent** `number` *required*
>
> Conversions divided by the threshold, as a percentage clamped to 0–100. The ad group's `delivery_status` remains authoritative.

**link\_clicks** `number` *required*

Page views on your destination from people who clicked the ad, recorded by the
Whop pixel. Counted the same way on every ad platform.

**purchase\_value** `number` *required*

USD value of pixel-attributed purchases.

**purchases** `number` *required*

Whop pixel-attributed purchases, last-click.

**reach** `number` *required*

The number of unique people who saw this.

**regions** `object` *required*

Locations targeted and excluded.

> [accordion] Properties
>
> **exclude** `object` *required*
>
> Locations excluded from targeting. Country groups can't be excluded.
>
> > [accordion] Properties
> >
> > **cities** `object[]` *required*
> >
> > Cities, each keyed by its US state or country and its name.
> >
> > > [accordion] Properties
> > >
> > > **key** `string` *required*
> > >
> > > The city's key: its US state or country, then its name, such as
> > > `US-TX:Austin`. An older ad group can hold a numeric key, which stays valid.
> > >
> > > **name** `string`
> > >
> > > City name, such as `Austin`. Absent when the ad group stores none.
> >
> > **countries** `string[]` *required*
> >
> > Countries, as ISO 3166-1 alpha-2 codes such as `US`.
> >
> > **country\_groups** `string[]` *required*
> >
> > Multi-country groups such as `worldwide` or `europe`.
> >
> > **custom\_locations** `object[]` *required*
> >
> > Circular areas, each a coordinate plus a radius.
> >
> > > [accordion] Properties
> > >
> > > **country\_code** `string`
> > >
> > > ISO 3166-1 alpha-2 country the point falls in. Send it under a special ad
> > > category, where the campaign must declare the countries its ad sets reach.
> > >
> > > **distance\_unit** `string` *required*
> > >
> > > Unit for `radius`.
> > >
> > > Available options: `mile`, `kilometer`
> > >
> > > **latitude** `number` *required*
> > >
> > > Latitude of the center point.
> > >
> > > **longitude** `number` *required*
> > >
> > > Longitude of the center point.
> > >
> > > **name** `string`
> > >
> > > Label for the location, such as a city or address. Absent when the location
> > > has no label.
> > >
> > > **radius** `number` *required*
> > >
> > > Radius around the center point, in `distance_unit`.
> >
> > **regions** `string[]` *required*
> >
> > US states and DC, as ISO 3166-2 codes such as `US-CA`. US territories (`PR`,
> > `GU`, `VI`, `AS`, `MP`) and everywhere outside the US are targeted through
> > `countries`.
> >
> > **zips** `string[]` *required*
> >
> > ZIP and postal codes, keyed as `COUNTRY:CODE`, such as `US:78756`.
>
> **include** `object` *required*
>
> Locations the ad group targets.
>
> > [accordion] Properties
> >
> > **cities** `object[]` *required*
> >
> > Cities, each keyed by its US state or country and its name.
> >
> > > [accordion] Properties
> > >
> > > **key** `string` *required*
> > >
> > > The city's key: its US state or country, then its name, such as
> > > `US-TX:Austin`. An older ad group can hold a numeric key, which stays valid.
> > >
> > > **name** `string`
> > >
> > > City name, such as `Austin`. Absent when the ad group stores none.
> >
> > **countries** `string[]` *required*
> >
> > Countries, as ISO 3166-1 alpha-2 codes such as `US`.
> >
> > **country\_groups** `string[]` *required*
> >
> > Multi-country groups such as `worldwide` or `europe`.
> >
> > **custom\_locations** `object[]` *required*
> >
> > Circular areas, each a coordinate plus a radius.
> >
> > > [accordion] Properties
> > >
> > > **country\_code** `string`
> > >
> > > ISO 3166-1 alpha-2 country the point falls in. Send it under a special ad
> > > category, where the campaign must declare the countries its ad sets reach.
> > >
> > > **distance\_unit** `string` *required*
> > >
> > > Unit for `radius`.
> > >
> > > Available options: `mile`, `kilometer`
> > >
> > > **latitude** `number` *required*
> > >
> > > Latitude of the center point.
> > >
> > > **longitude** `number` *required*
> > >
> > > Longitude of the center point.
> > >
> > > **name** `string`
> > >
> > > Label for the location, such as a city or address. Absent when the location
> > > has no label.
> > >
> > > **radius** `number` *required*
> > >
> > > Radius around the center point, in `distance_unit`.
> >
> > **regions** `string[]` *required*
> >
> > US states and DC, as ISO 3166-2 codes such as `US-CA`. US territories (`PR`,
> > `GU`, `VI`, `AS`, `MP`) and everywhere outside the US are targeted through
> > `countries`.
> >
> > **zips** `string[]` *required*
> >
> > ZIP and postal codes, keyed as `COUNTRY:CODE`, such as `US:78756`.

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

**starts\_at** `string | null` *required*

When the ad group starts delivering, as an ISO 8601 timestamp. `null` when it
starts as soon as it's active.

**status** `string` *required*

Whether the ad group is enabled. `active` and `paused` are set by you; `rejected` means it failed ad review; `duplicating` is a copy still being filled in.

Available options: `active`, `paused`, `rejected`, `duplicating`

**submitted\_application\_value** `number` *required*

USD value attributed to submit-application events. Sums the value sent with
each event, normalized to USD; events without a value contribute 0.

**submitted\_applications** `number` *required*

Whop pixel-attributed submit-application events, last-click.

**title** `string | null` *required*

Display name of the ad group.

**unique\_click\_through\_rate** `number | null` *required*

Unique clicks divided by impressions, between 0 and 1.

**unique\_clicks** `number` *required*

People who clicked, reported by the Whop pixel, counted once per person.

**updated\_at** `string` *required*

When the ad group was last updated, as an ISO 8601 timestamp.

**viewed\_content\_value** `number` *required*

USD value attributed to view-content events. Sums the value sent with each
event, normalized to USD; events without a value contribute 0.

**viewed\_contents** `number` *required*

Whop pixel-attributed view-content events, last-click.

```json AdGroup theme={null}
{
	"id": "adgrp_xxxxxxxxxxx",
	"ad_campaign": {
		"id": "adcamp_xxxxxxxxxx"
	},
	"added_to_cart_value": 1050,
	"added_to_carts": 24,
	"audiences": {
		"exclude": [],
		"include": ["adaud_xxxxxxxxxxx"]
	},
	"bid_type": "minimum_cost",
	"budget_amount": 100,
	"budget_amount_local": 100,
	"budget_currency": "usd",
	"budget_type": "daily",
	"click_through_rate": 0.036,
	"clicks": 720,
	"completed_registration_value": 0,
	"completed_registrations": 16,
	"contact_value": 0,
	"contacts": 10,
	"conversion_event": "purchase",
	"cost_per_added_to_cart": 8.33,
	"cost_per_click": 0.28,
	"cost_per_completed_registration": 12.5,
	"cost_per_contact": 20,
	"cost_per_lead": 13.33,
	"cost_per_mille": 10,
	"cost_per_purchase": 25,
	"cost_per_result": 25,
	"cost_per_schedule": null,
	"cost_per_submitted_application": null,
	"cost_per_unique_click": 0.33,
	"cost_per_viewed_content": 0.67,
	"created_at": "2026-06-01T12:00:00Z",
	"custom_conversions": 5,
	"custom_event_counts": {
		"QualifiedCall": 4,
		"BookedDemo": 1
	},
	"custom_event_values": {
		"QualifiedCall": 2000,
		"BookedDemo": 0
	},
	"delivery_status": "issues",
	"learning_progress": null,
	"demographics": {
		"automatic": false,
		"minimum_age": 21,
		"maximum_age": 45,
		"gender": "all"
	},
	"delivery_schedule": {
		"monday": [
			{
				"start": "09:00",
				"end": "17:00"
			}
		],
		"tuesday": [
			{
				"start": "09:00",
				"end": "17:00"
			}
		],
		"wednesday": [
			{
				"start": "09:00",
				"end": "17:00"
			}
		],
		"thursday": [
			{
				"start": "09:00",
				"end": "17:00"
			}
		],
		"friday": [
			{
				"start": "09:00",
				"end": "12:00"
			},
			{
				"start": "16:00",
				"end": "20:00"
			}
		],
		"saturday": [
			{
				"start": "10:00",
				"end": "14:00"
			}
		],
		"sunday": [
			{
				"start": "12:00",
				"end": "16:00"
			}
		]
	},
	"desired_cost_per_result": 30,
	"devices": {
		"platforms": ["mobile", "desktop"],
		"operating_systems": [
			{
				"os": "ios",
				"minimum_version": "18.0"
			}
		]
	},
	"ends_at": "2026-07-01T12:00:00Z",
	"frequency": 1.6,
	"impressions": 20000,
	"issues": [
		{
			"id": "adiss_xxxxxxxxxxx",
			"category": "error",
			"title": "Ad issue",
			"message": "The audience is too narrow to deliver. Broaden the targeting to reach more people.",
			"resource_id": "adgrp_xxxxxxxxxxx",
			"resource_type": "ad_group"
		}
	],
	"languages": ["en"],
	"lead_value": 750,
	"leads": 15,
	"link_clicks": 576,
	"purchase_value": 5200,
	"purchases": 8,
	"reach": 12500,
	"regions": {
		"include": {
			"countries": ["US"],
			"country_groups": [],
			"regions": ["US-CA"],
			"cities": [
				{
					"key": "2418779",
					"name": "Miami"
				}
			],
			"zips": [],
			"custom_locations": [
				{
					"latitude": 30.2672,
					"longitude": -97.7431,
					"radius": 10,
					"distance_unit": "mile",
					"name": "Austin, TX"
				}
			]
		},
		"exclude": {
			"countries": [],
			"country_groups": [],
			"regions": [],
			"cities": [
				{
					"key": "2421215",
					"name": "Anchorage"
				}
			],
			"zips": [],
			"custom_locations": [
				{
					"latitude": 40.758,
					"longitude": -73.9855,
					"radius": 2,
					"distance_unit": "mile",
					"name": "Times Square"
				}
			]
		}
	},
	"result_event": "purchase",
	"result_event_name": null,
	"results": 8,
	"return_on_ad_spend": 26,
	"schedule_value": 0,
	"schedules": 0,
	"spend": 200,
	"spend_currency": "usd",
	"starts_at": "2026-06-01T12:00:00Z",
	"status": "active",
	"submitted_application_value": 0,
	"submitted_applications": 0,
	"title": "Austin ceramic coating customers",
	"unique_click_through_rate": 0.03,
	"unique_clicks": 600,
	"updated_at": "2026-06-02T12:00:00Z",
	"viewed_content_value": 0,
	"viewed_contents": 300
}
```


## Related topics

- [Ad Groups](https://docs.whop.com/api-reference/legacy/ad-groups/ad-group.md)
- [Delete an Ad Group](https://docs.whop.com/api-reference/ad-groups/delete-an-ad-group.md)
- [Duplicate an Ad Group](https://docs.whop.com/api-reference/ad-groups/duplicate-an-ad-group.md)
