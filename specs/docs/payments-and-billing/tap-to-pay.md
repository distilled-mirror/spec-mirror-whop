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

# Tap to Pay on iPhone

> Accept in-person contactless payments right on your iPhone with the Whop app. No card reader or extra hardware required.

Tap to Pay on iPhone lets you accept in-person payments directly on your iPhone using the Whop iOS app. Customers hold a contactless card or their phone over your iPhone and the payment clears in seconds. There's no card reader, no dock, and no extra hardware to buy.

> [card] No hardware required
>
> Your iPhone is the card reader. Nothing else to buy or carry.

> [card] One balance
>
> In-person sales land in the same Whop balance as your online sales.

> [card] Contactless
>
> Accept contactless cards, Apple Pay, and other mobile wallets.

> [!NOTE]
>
> You'll need an **iPhone XS or later** running **iOS 18 or later**, plus the
> latest [Whop iOS app](https://apps.apple.com/app/apple-store/id1600181492?pt=123864738\&ct=docs_tap_to_pay\&mt=8).
> Your business address must be in a [supported country](#where-its-available).
> You also need an internet connection. Tap to Pay isn't available offline.

## Where it's available

Availability follows the country on your **business address**, which you set under **Settings** → **Tax** on Whop. Tap to Pay on iPhone currently works for businesses based in:

| Country | In-person currency |
| - | - |
| Australia | <abbr title="Australian dollars">AUD</abbr> |
| Canada | <abbr title="Canadian dollars">CAD</abbr> |
| Poland | <abbr title="Polish złoty">PLN</abbr> |
| United Kingdom | <abbr title="British pounds">GBP</abbr> |
| United States | <abbr title="United States dollars">USD</abbr> |

An in-person sale settles in your business's own country, so Whop needs a payments entity there. Poland is the only European Union country available today.

> [!WARNING]
>
> In every other country, Whop declines an in-person charge — even where Apple
> offers Tap to Pay. Share the checkout link or let the customer scan the QR code instead.

## How it works

**1. Create a product**

In the Whop app, add a product for whatever you're selling in person. A
single general product works well for a shop or stall.

**2. Create a checkout link**

When it's time for a customer to pay, create a checkout link for the
product from your iPhone. A **Tap to pay** button appears below the QR code
on the checkout screen.

![Whop iOS checkout screen showing a product QR code with Copy link, Share link, and Tap to pay buttons](https://mintcdn.com/whop/SLzd5Nz08GLJ309O/images/tap-to-pay-checkout.png?fit=max\&auto=format\&n=SLzd5Nz08GLJ309O\&q=85\&s=93a79786375614a21e45e88152380d15)

**3. Tap the button and take the payment**

Tap **Tap to pay**, then have the customer hold their contactless card,
iPhone, or Apple Watch to the top of your iPhone. The payment clears in
seconds.

**4. Send a receipt (optional)**

The customer can add their email at checkout to receive a receipt. The sale
shows up alongside your other payments in the Whop dashboard.

> [!TIP]
>
> To let customers pay themselves, share the checkout link or let them
> scan the QR code instead of tapping. Both flows settle to the same balance.
> Turn on **Point of Sale** in **Settings** → **Tap to Pay**
> to charge from your product list or a custom amount.
> You can also **Ask for a tip** before the customer presents their card.

## Charge from a link

A Tap to Pay link opens the Whop app straight on the Tap to Pay screen for one checkout link. Send it to your team so anyone who takes payments for your business can charge it from their own iPhone.

To get the link, open the checkout link in the Whop app, tap the menu in the top right corner, and choose **Copy Tap to Pay link** or **Share Tap to Pay link**. It's the checkout link with `?ttp=1` added:

```text theme={null}
https://whop.com/checkout/plan_XXXXXXXXXXXXXX?ttp=1
```

* **On an iPhone with the Whop app**, signed in as a team member who can take payments for the business, the link opens Tap to Pay for that checkout link.
* **Everywhere else**, it opens the normal checkout page, where the customer can pay online.

### Add metadata to a sale

An integration can tag an in-person sale by adding `metadata[key]=value` parameters to the link:

```text theme={null}
https://whop.com/checkout/plan_XXXXXXXXXXXXXX?ttp=1&metadata[order_id]=1234&metadata[table]=7
```

A payment taken with Tap to Pay from that link carries those values in its `metadata`, on the payment in the API and in payment webhooks.

* You can add up to 50 keys. A key can be up to 100 characters and a value up to 500.
* If Whop rejects the metadata, the sale fails with the reason and the customer isn't charged.
* Only Tap to Pay sales carry the metadata. A customer who pays the same link in a browser creates a payment without it.
* Anyone who has the link can change the values, so don't put anything in them that you need to trust.

## What you can accept

* Accept **contactless cards** (Visa, Mastercard, American Express, Discover)
* **Apple Pay** and other mobile wallets
* **Apple Watch** and phone-based contactless payments

> [!WARNING]
>
> Tap to Pay supports **one-time payments only**. It can't start subscriptions or
> recurring plans.

## Fees and payouts

Tap to Pay sales settle to your Whop balance just like online payments. Whop
pays them out through your normal payout method and schedule. For per-transaction
pricing, see the [Fees](https://docs.whop.com/payments-and-billing/fees/fees.md) page.

## Related resources

> [card] [Create a checkout link](https://docs.whop.com/manage-your-business/payment-processing/create-checkout-link.md)
>
> Set up the checkout link customers pay through in person.

> [card] [Brick and mortar](https://docs.whop.com/supported-business-models/brick-and-mortar.md)
>
> A full walkthrough for running in-store sales on Whop.


## Related topics

- [Brick and Mortar](https://docs.whop.com/supported-business-models/brick-and-mortar.md)
- [Whop Partner Program Guide](https://docs.whop.com/partners.md)
- [Checkout Branding](https://docs.whop.com/manage-your-business/payment-processing/checkout-branding.md)
