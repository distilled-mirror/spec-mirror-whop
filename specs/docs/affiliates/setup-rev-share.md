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

# Set Up Revenue Share

> Split revenue with partners on each transaction, with their share deposited into their Whop balance.

Use **revenue share affiliates** for **strategic partnerships** that split revenue at the **transaction level** instead of using [traditional referral links](https://docs.whop.com/affiliates/promote-your-business.md).

With revenue share, you can define **who receives a portion of revenue** for each product. Whop calculates the split **after applying all Whop fees**, which provides clear and accurate revenue sharing. When a transaction completes, Whop Payments deposits the partner's portion **into their Whop balance** without manual processing.

## How to set up a revenue share partner

### 1. Go to the Affiliates section of your dashboard, under Marketing

![Affiliates under Marketing](https://mintcdn.com/whop/lB_LXyTVrLvv_lYf/images/affiliates-marketing-section.png?fit=max\&auto=format\&n=lB_LXyTVrLvv_lYf\&q=85\&s=5192f9ef37cf5f3a83f099bff73b6c38)

### 2. Select 'Share your revenue'

![Commission settings](https://mintcdn.com/whop/uAJC0Xw6_CxdozEK/images/affiliates-rev-share-step-3.png?fit=max\&auto=format\&n=uAJC0Xw6_CxdozEK\&q=85\&s=113e1abbbde82cacf998c6e481ea546b)

### 3. Complete invite popup

![Commission settings](https://mintcdn.com/whop/uAJC0Xw6_CxdozEK/images/affiliates-rev-share-step-4.png?fit=max\&auto=format\&n=uAJC0Xw6_CxdozEK\&q=85\&s=9ccc66085e56432a51bbde50818cb7e7)

* **User** – You can add users by their email, Whop username, or Whop user
  ID. - **Share %** - The percentage the user should receive of all products.
* **Products** - Override by product the percentage the user should receive
  of the product.


## Related topics

- [Set Up Pricing](https://docs.whop.com/manage-your-business/payment-processing/set-up-pricing.md)
- [Set Up Payouts](https://docs.whop.com/manage-your-business/manage-payouts/set-up-payouts.md)
- [Direct-to-consumer E-commerce](https://docs.whop.com/supported-business-models/dtc-ecommerce.md)
