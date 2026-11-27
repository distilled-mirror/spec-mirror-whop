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

# In-app iOS Purchases

> How Apple's fees affect pricing and payouts when customers buy through the Whop iOS app.

## Purchases processed in the app

Apple processes one-time purchases made through the Whop iOS app with its in-app purchasing system, up to a price limit. The limit depends on [who pays Apple's fee](#choose-who-pays-the-fee):

* **Buyer pays**: one-time purchases **under <abbr title="United States dollars">USD</abbr> 700**. With the 42.86% markup, that's the highest price that still fits under Apple's top price point of <abbr title="United States dollars">USD</abbr> 1,000.
* **You pay**: one-time purchases **up to <abbr title="United States dollars">USD</abbr> 1,000**, Apple's top price point.

One-time purchases over the limit use Whop's checkout and don't go through the app. Whop converts prices in other currencies to <abbr title="United States dollars">USD</abbr> to check the limit.

This means:

* Apple charges a 30% fee on these purchases.
* Apple collects the fee — Whop doesn't.

**Purchases not processed by Apple:**

* One-time purchases over the limit for your setting: <abbr title="United States dollars">USD</abbr> 700 or more with **Buyer pays**, or more than <abbr title="United States dollars">USD</abbr> 1,000 with **You pay**
* All subscription purchases

Whop's web checkout system processes these purchases without the 30% App Store fee.

## Required platform fee

The 30% fee isn't optional.

* Apple requires all apps on the App Store to use their in-app purchasing system for eligible transactions.
* Neither Whop nor individual customers can opt out of this system.

Using the iOS app is optional. To avoid the Apple fee, customers can make purchases using:

* Any desktop or mobile web browser
* The Whop website instead of the iOS app

## Recipient of the 30% Apple fee

Apple receives the 30% fee — Whop doesn't.

You choose whether the buyer or your business pays it. See [Choose who pays the fee](#choose-who-pays-the-fee).

## Choose who pays the fee

Set who pays Apple's fee in your dashboard:

1. Go to **Settings** > **Payments**.
2. Find the **Apple in-app purchase fee** row in the **Processing** group.
3. Select **Buyer pays** or **You pay**.

The setting applies to new purchases in the iOS app.

### Buyer pays (default)

To keep your earnings at 100%, Whop **increases the price** of one-time products under <abbr title="United States dollars">USD</abbr> 700 on the iOS app only. This price increase offsets the 30% fee by marking up the product by **42.86%**.

Whop bases the markup on this formula: 1 / (1 - 0.30) = 1.4286 (a 42.86% increase).

So a product that normally costs <abbr title="United States dollars">USD</abbr> 100 appears as about <abbr title="United States dollars">USD</abbr> 142.86 on the iOS app. Apple sells at set price points, so the iOS price rounds up to the nearest one.

### You pay

Buyers see your regular price on the iOS app for one-time products up to <abbr title="United States dollars">USD</abbr> 1,000, rounded up to the nearest Apple price point. Apple keeps 30% of the sale price, and your payout comes from the rest.

To change this setting through the API, set `apple_iap_fee_payer` to `buyer` or `company` with [Update Account Preferences](https://docs.whop.com/api-reference/accounts/update-account-preferences.md). [Retrieve Account Preferences](https://docs.whop.com/api-reference/accounts/retrieve-account-preferences.md) returns the current value.

## Payout timing

Apple processes all iOS purchases before remitting payments to Whop. The timeline is:

* **Up to 75 days** after the transaction date
* While processing, the transaction will show as **pending** in your Whop dashboard

## Supported currencies

Apple's in-app purchasing system supports:

* 44+ global currencies
* 200+ payment methods

For a complete list, see [Apple's developer documentation](https://developer.apple.com/documentation/storekit).

## Refunds for in-app purchases

Customers who purchase through Apple in-app purchases must:

* Request refunds **directly through Apple**
* Follow [Apple's refund policy and process](https://www.apple.com/legal/internet-services/itunes/)

## Handling customer concerns

With **Buyer pays**, prices on the Whop iOS app are higher, which may confuse or frustrate some customers.

If that happens, you can clarify:

* The increased price is due to Apple's mandatory 30% App Store fee.
* The same product is available at the normal price on the Whop website or in any browser.

This helps with transparency and gives customers a clear workaround. To show iOS buyers your regular price instead, select **You pay**.


## Related topics

- [Whop Pricing and Fees](https://docs.whop.com/payments-and-billing/fees/fees.md)
- [Access Content on Mobile](https://docs.whop.com/memberships-and-access/access-discord-server/access-content-on-mobile.md)
- [Brick and Mortar](https://docs.whop.com/supported-business-models/brick-and-mortar.md)
