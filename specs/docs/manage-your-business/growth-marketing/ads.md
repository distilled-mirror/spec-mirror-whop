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

# Whop Ads

> Run ads through Whop-owned ad accounts with first-party attribution from real buyer data

Whop Ads gives creators and businesses external advertising infrastructure. It includes Whop-owned ad accounts on major ad networks and Whop's first-party attribution pixel ([the Whop Pixel](#whop-attribution-pixel)). You run everything from one dashboard inside Whop. **No setup fees, no monthly fees, no minimum spend.**

You bring the strategy and creative — Whop Ads is infrastructure, not a managed service. Your media buyer (or you) still runs the campaigns.

## How Whop Ads compares to running your own ad account

| | Your own ad account | Whop Ads |
| - | - | - |
| **Account standing** | Standard | High-standing Whop accounts — priority bidding and lower cost per thousand impressions at scale |
| **Ad rejections** | Common | Fewer thanks to higher account standing |
| **Support** | Standard platform support | Whop account manager |
| **Bulk uploads** | Limited | Launch dozens of ads in minutes |
| **Daily spend** | Hard platform limits | No spend limit — scale from day one |
| **Attribution** | Browser-based, degrading as cookies disappear | First-party attribution from real payment data (the Whop Pixel) |

## Pricing

It's free to use Whop Ads. There are no platform, setup, or monthly fees when you run ads through the Whop dashboard — you only pay for ad spend, and you can fund it three ways:

| Funding method | Processing fee | Rewards |
| - | - | - |
| **Credit card** | None | Earn your card's normal points/rewards |
| **Whop Card** | None | Spend directly from your Whop balance |
| **Pending Whop balance** | None | Spend unsettled revenue same-day instead of waiting 4–5 days |

## Get started

### 1. Open the Ads tab

Open the **Ads** tab in your Whop business dashboard. There's no application or waitlist — every business can launch ads today.

### 2. Connect your accounts

Connect your social accounts via OAuth. Whop automatically provisions a pixel for your business, seeded with your existing Whop sales data.

### 3. Import custom audiences (optional)

Bring your custom audiences over from your existing ad account into your Whop ad account.

### 4. Run the self-serve setup

Self-serve setup takes under 5 minutes, and you can launch your first campaign right away.

> [!TIP]
>
> Invite your media buyer or team with the **Advertiser** role on Whop. They get access to the Ads dashboard only — not your analytics, payouts, or business settings.

## Whop attribution pixel

The Whop Pixel is Whop's first-party attribution pixel. Whop owns the underlying payments stack, so the pixel attributes conversions from real payment data instead of browser signals. This keeps attribution accurate as third-party cookies disappear.

[Install the Whop Pixel on your site](https://docs.whop.com/developer/ads/pixel.md) to start tracking conversions.

**Official integrations:**

* Everflow
* Triple Whale
* Cometly
* Trackbee
* Cortana
* Omni
* Hyros
* WeTracked

**Send conversions in and out:**

* Whop forwards your conversions to the ad networks your campaigns run on.
* Any platform that can fire events to an endpoint can report conversions to Whop through the [Events API](https://docs.whop.com/api-reference/events/create-event.md). Examples include sales platforms, funnel builders, form builders, and custom backends.

## Lookalike audiences

> [!NOTE]
>
> Lookalike audiences built from Whop's first-party buyer data are coming soon.

## Fund ad campaigns with Whop

Fund campaigns the same day from your **pending Whop balance** instead of waiting the usual 4–5 days for revenue to settle. This is especially useful when you're scaling fast and don't want cash flow to gate ad spend.

The **Whop Card** draws from your Whop balance, so you can put the revenue you earn on Whop directly toward ad spend.

You don't need a Whop Card to start — a regular credit card works on day one. Your account manager can walk you through getting set up with one whenever you're ready. [Learn more about Whop Cards](https://docs.whop.com/whop-finance/cards.md).

## Spend limits

There's no spend limit on Whop's ad accounts. You can scale from day one. Whop's account standing supports high daily spend without the hard platform caps of a standard account.

## Where your ads run

Whop runs your campaigns on the ad networks it partners with.

> [!NOTE]
>
> Manage campaigns inside the Whop dashboard, not in an ad network's own ads manager. The Whop Pixel and cross-platform features live in the Whop UI, which provides a consistent experience across every network. AI creative tools are coming soon.

## Who can advertise

Whop Ads supports legitimate ("white hat") businesses across most categories. Before you launch a campaign, Whop Ads runs **built-in compliance checks** to make sure your creatives and copy meet ad network requirements.

| Allowed | Not allowed |
| - | - |
| Trading and finance education | Fake or unverifiable income claims |
| Sports picks and handicapping | Fake testimonials |
| E-commerce | Lewd or adult content |
| Business opportunity and coaching | Scam-style or deceptive offers |
| Most information products | |

## Limits to know about

* One ad account per Whop business
* No credit lines — fund ad spend through a credit card, Whop Card, or pending Whop balance

## Next steps

> [card] [Tracking links](https://docs.whop.com/manage-your-business/growth-marketing/tracking-links.md)
>
> Build branded links to attribute traffic and conversions across channels.

> [card] [Promo codes](https://docs.whop.com/manage-your-business/growth-marketing/promo-codes.md)
>
> Run discounts and trials to convert paid traffic into paying members.

> [card] [Whop Cards](https://docs.whop.com/whop-finance/cards.md)
>
> Fund ad spend straight from your Whop balance and reinvest into your business.


## Related topics

- [Whop Partner Program Guide](https://docs.whop.com/partners.md)
- [Tracking Integrations](https://docs.whop.com/manage-your-business/growth-marketing/tracking-integrations.md)
- [Create a Whop](https://docs.whop.com/payments/create-a-whop.md)
