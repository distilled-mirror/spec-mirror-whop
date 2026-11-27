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

# Create a Whop

> Create your Whop account at whop.com/sell, choose whether to sell digital products or use Whop for payment processing only, and name your business.

To start accepting payments, create your account on Whop.

### 1. Go to whop.com/sell

* Head over to [**whop.com/sell**](https://whop.com/sell)
* Select **Start selling**

### 2. Choose how you plan to use Whop

When asked how you plan to use Whop, you will see two options:

1. **Build & sell digital products on Whop:** Choose this option if you plan to build, host, and sell digital products while accepting payments through Whop
2. **Use Whop for payment processing only:** Choose this option if you want to use Whop to process payments and create checkout links only - but host and deliver your product elsewhere

If you want to use Whop for payment processing only and host your digital product elsewhere, select **Use Whop for payment processing only**. Then select **Next**.

### 3. Name your business

* Choose a name for your business

- Select **Create my business**

## Next steps

To start accepting payments through Whop, make sure you've completed the rest of the setup steps:

> [card] [Create a product](https://docs.whop.com/payments/create-a-product.md)
>
> Products are how you package and price your various offers.

> [card] [Copy checkout link](https://docs.whop.com/payments/create-checkout-link.md)
>
> Checkout links let you send customers straight to a payment page.


## Related topics

- [Create a Waitlist](https://docs.whop.com/manage-your-business/products/create-waitlist.md)
- [Educational Programs](https://docs.whop.com/supported-business-models/educational-programs.md)
- [Paid Groups](https://docs.whop.com/supported-business-models/paid-groups.md)
