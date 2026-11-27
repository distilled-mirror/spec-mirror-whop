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

# Direct-to-consumer E-commerce

> Sell physical products on Whop: create a product for your item, set its price, set up fulfillment, and share your checkout link with buyers.

Whop continues to improve support for selling physical products. In the meantime, many creators use Whop to accept payments and bring social features to their stores.

## Sell a physical product on Whop in 4 simple steps

### 1. Create a product for your item

Go to your whop > select **Products** > **Add product**. Name your product
and write a catchy headline so buyers know what to expect.

### 2. Pick your price

Choose a one-time price. If you offer refills or memberships, you can also
use recurring pricing. You can always adjust later.

### 3. Set up fulfillment

Set up webhooks with [**Zapier**](https://zapier.com) to trigger your order
fulfillment (e.g., create a shipment, notify your 3PL, or send confirmation
emails). In the meantime, use custom fields on the pricing option to collect
what you need to fulfill (e.g., shipping address, size, color).

### 4. Share your link with buyers

Copy your product checkout link and share it anywhere—your website, socials,
email, or DMs.

> [!TIP]
>
> Encourage community and support: add the
> [**Chat**](https://whop.com/apps/app_xml5hbizmZPgUT/install/) app and grant
> access to everyone who bought this product so owners can get updates, ask
> questions, and share feedback.


## Related topics

- [Consumer Apps](https://docs.whop.com/whop-apps/consumer-apps.md)
- [Set Up Payouts](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md)
- [Whop Ads](https://docs.whop.com/manage-your-business/growth-marketing/ads.md)
