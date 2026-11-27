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

# Launch Your Business

> Create your Whop account, choose between selling digital products or payment processing only, name your business, then add apps and set up pricing.

To launch your online business on Whop, you first need to make an account.

### 1. Go to whop.com/sell

* Head over to [**whop.com/sell**](https://whop.com/sell)
* Select **Start selling**

### 2. Choose how you plan to use Whop

Select the option that matches how you plan to use Whop

1. **Build & sell digital products on Whop:** Choose this option if you plan to use Whop for building, hosting, and selling your digital products and accepting payments.
2. **Use Whop for payment processing only:** Choose this option if you want to use Whop to process payments and create checkout links only - but host and deliver your product elsewhere.

Select the option that best fits how you plan to use Whop, then select **Next**.

### 3. Name your business

Choose a name for your business.

> [!TIP]
>
> You can always change this later.

## Next steps

To launch your internet business, make sure you’ve completed the rest of the setup steps:

> [card] [Add apps to your whop](https://docs.whop.com/add-apps.md)
>
> Choose from dozens of apps to create exactly what you need.

> [card] [Set up pricing](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
>
> Choose your pricing model and what members get when they join.


## Related topics

- [Get Started with Whop](https://docs.whop.com/get-started.md)
- [Educational Programs](https://docs.whop.com/supported-business-models/educational-programs.md)
- [Promote Your Business](https://docs.whop.com/affiliates/promote-your-business.md)
