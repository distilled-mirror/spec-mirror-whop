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

# GoHighLevel

> Use Whop as a native payment provider in GoHighLevel for payment links, checkout pages, invoices, and subscriptions.

> [card] [Connect Whop to GoHighLevel](https://connectv2.whopintegration.com/)
>
> Open the setup wizard

The Whop Payment Integration makes Whop a payment provider inside GoHighLevel. Customers pay on your GoHighLevel payment links, checkout pages, and invoices, and the payments run through your Whop business.

You need access to the GoHighLevel agency or sub-accounts you want to connect, and admin access to your Whop business. Sign in to both before you start.

## What it does

| Feature | What it means |
| - | - |
| **Native checkout** | Whop is a GoHighLevel payment provider. Customers pay without leaving your funnel or invoice. |
| **One-time payments** | Payment links, checkout pages, and invoices |
| **Subscriptions** | Renewals, cancellations, and status changes stay in sync between Whop and GoHighLevel |
| **Saved cards** | Charge a saved card for one-click upsells and lower-priced offers |
| **Refunds** | Full and partial refunds |
| **Real-time updates** | Captured, failed, and refunded payments post to GoHighLevel as they happen |

## Set up the integration

The setup wizard has five steps.

**1. Connect GoHighLevel**

Open the wizard with the button at the top of this page and select **Onboard GHL account**. GoHighLevel asks you to sign in and authorize the app on a sub-account or your agency. Check that you've picked the right agency and sub-account before you approve.

**2. Connect Whop**

Sign in to Whop and choose the Whop business that should receive payments. Then select which GoHighLevel sub-accounts to connect.

**3. Choose payment methods**

Pick the payment methods customers can use.

**4. Join support**

Get access to the integration's support community.

**5. Set Whop as the default provider**

In each connected GoHighLevel sub-account, go to **Payments** → **Integrations** and make Whop the default provider. GoHighLevel uses one default provider at a time, so payments don't route through Whop until you do this.

## Troubleshooting

**Connected the wrong agency or sub-account**

Sign out of GoHighLevel, sign back in with the right account, and restart the wizard.

**Payments aren't reaching Whop**

In the sub-account, go to **Payments** → **Integrations** and check that Whop is the default provider.

## Next steps

> [card] [HubSpot](https://docs.whop.com/third-party-integrations/hubspot.md)
>
> Sync Whop payments into HubSpot.

> [card] [Pricing and fees](https://docs.whop.com/fees.md)
>
> See which payment methods Whop supports.


## Related topics

- [Install the Whop Pixel on GoHighLevel](https://docs.whop.com/developer/ads/platforms/gohighlevel.md)
- [HubSpot](https://docs.whop.com/third-party-integrations/hubspot.md)
- [Install the Whop Pixel](https://docs.whop.com/developer/ads/pixel.md)
