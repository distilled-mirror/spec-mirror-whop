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

# Brick and Mortar

> Take in-person payments in your shop with the Whop iOS app and give paying customers extra perks.

Whop continues to improve support for brick-and-mortar businesses. In the meantime, many shops use Whop to accept in-person payments and add social features for their patrons.

## Run in-store sales on Whop in 4 simple steps

### 1. Create a general product for your in-store purchase

Go to your whop > select **Products** > **Add product**. Name the product
your store and write a catchy headline as to why people come to your shop.

### 2. Take in-person payments with the Whop iOS app

Open the Whop iOS app, select the bottom right tab, and then create a
checkout link when it's time for a customer to pay. After you create a
checkout link from your phone, a QR code appears so customers can
scan and pay. A **Tap to pay** button lets you take a
contactless card or wallet on the iPhone itself. See [Tap to Pay on iPhone](https://docs.whop.com/payments-and-billing/tap-to-pay.md)
for countries, device requirements, and the in-person flow. You can also save or print the QR for signage.

### 3. Give patrons exclusive perks

Add a patrons-only
[**Forums**](https://whop.com/apps/app_dYfm2IdXhDMquv/install/) app for
updates and announcements, and add a merch store using the [**Merch
Store**](https://whop.com/apps/app_bLSc10GEFPDu4G/install/) app so paying
customers can buy your merchandise.

> [!TIP]
>
> This is just one good way to use Whop as a brick-and-mortar business. There
> are many other setups depending on your goals.


## Related topics

- [Tap to Pay on iPhone](https://docs.whop.com/payments-and-billing/tap-to-pay.md)
- [Get Started with Whop](https://docs.whop.com/get-started.md)
