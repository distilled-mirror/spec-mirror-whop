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

# HubSpot

> Sync Whop payments and subscriptions into HubSpot and send Whop checkout links from deals and contacts.

> [card] [Install Whop Payments for HubSpot](https://ecosystem.hubspot.com/marketplace/listing/whoppayments-by-whop)
>
> Available on the HubSpot App Marketplace

The Whop Payments app brings your Whop revenue into HubSpot as native Commerce Hub records. Your sales team sees payments and subscriptions on the contact and deal, and can send a Whop checkout link without leaving HubSpot.

You need a Whop business with at least one product, and a HubSpot account where you can install apps.

## What syncs

| Whop data | In HubSpot |
| - | - |
| **Payments** | Commerce Hub payment records, associated with the contact, deal, quote, or invoice |
| **Memberships** | Subscription records with status and recurring revenue, associated with the contact and deal |
| **Products and plans** | Products in your HubSpot product library |

The app syncs **every** Whop payment, not only ones that started in HubSpot. Each payment shows whether it came from a HubSpot checkout link or from elsewhere on Whop.

## Connect Whop to HubSpot

**1. Install the app**

Select **Install Whop Payments for HubSpot** at the top of this page, or search for "Whop" in the HubSpot App Marketplace. Choose the HubSpot account to install it in.

**2. Create a Whop API key**

In your Whop dashboard, go to **Developer**. Under **Account API keys**, select **Create API key**. Give the key the Admin role, name it (for example, "HubSpot"), and copy it. Whop shows the key only once.

**3. Paste the key into HubSpot**

Paste your Whop API key when the app asks for it, then save. To replace the key later, open the app's settings in HubSpot.

**4. Sync your products**

From the app's settings in HubSpot, sync your Whop products into the product library. Run it again after you add or change products in Whop.

Treat the API key like a password. If it leaks, delete it in your Whop dashboard and create a new one.

## Send checkout links from HubSpot

Add the Whop card (**Whop-Payment-Link-Deal**) to your deal and contact record views. From a record, pick a product and pricing plan in the card to generate a Whop checkout link for that buyer. When they pay, HubSpot links the payment and any subscription to that record.

You can also show the Whop payment link as a column in your HubSpot invoice list, and add a "Pay with Whop" button to quote templates. The app's setup guide, linked from the HubSpot listing, covers both.

The checkout shows the payment methods available in your Whop business for the buyer's location. See [Pricing and fees](https://docs.whop.com/fees.md) for the full list.

## Get help

For help with the app inside HubSpot, use the support link on the [HubSpot listing](https://ecosystem.hubspot.com/marketplace/listing/whoppayments-by-whop). For questions about payments or your Whop account, contact Whop support.

## Next steps

> [card] [GoHighLevel](https://docs.whop.com/third-party-integrations/gohighlevel.md)
>
> Use Whop as a payment provider in GoHighLevel.

> [card] [Zapier](https://docs.whop.com/third-party-integrations/zapier.md)
>
> Send Whop events to other apps and tools.


## Related topics

- [GoHighLevel](https://docs.whop.com/third-party-integrations/gohighlevel.md)
- [Zapier](https://docs.whop.com/third-party-integrations/zapier.md)
- [List Markets](https://docs.whop.com/api-reference/trades/list-markets.md)
