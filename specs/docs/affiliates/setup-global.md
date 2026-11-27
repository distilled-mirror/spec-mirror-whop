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

# Set Up Global Affiliates

> Global affiliates promote your product across the Whop marketplace at a default 30% commission. Change the Global and Member Affiliate Rate per product.

Whop uses **global affiliates** by default to promote your business across the **Whop marketplace**. After you list your product, any affiliate in Whop's network can promote it for a default **30%** commission without extra setup.

Affiliates can get a **unique referral link** and start promoting your business right away. These links track conversions and payouts, which can increase your exposure and sales without additional effort.

You can also choose a **custom global commission rate** for **users who have already purchased a membership** to your existing offer. This gives your most engaged customers an opportunity to become affiliates themselves.

## How to change your affiliate commissions

### 1. Go to the Affiliates section of your dashboard, under Marketing

![Affiliates under Marketing](https://mintcdn.com/whop/lB_LXyTVrLvv_lYf/images/affiliates-marketing-section.png?fit=max\&auto=format\&n=lB_LXyTVrLvv_lYf\&q=85\&s=5192f9ef37cf5f3a83f099bff73b6c38)

### 2. Select 'Set the affiliate commission for a specific product'

![Select affiliate commission](https://mintcdn.com/whop/uAJC0Xw6_CxdozEK/images/affiliates-global-step-3.png?fit=max\&auto=format\&n=uAJC0Xw6_CxdozEK\&q=85\&s=f06b3249f9b4b641889274b52e2647d0)

### 3. Set your product rates

![Commission settings](https://mintcdn.com/whop/uAJC0Xw6_CxdozEK/images/affiliates-global-step-4.png?fit=max\&auto=format\&n=uAJC0Xw6_CxdozEK\&q=85\&s=dfdba25fcfb3585684478c9c749abf04)-
**Types of rates:** - **Global Affiliate Rate** – The percentage of each
sale paid to affiliates who haven’t purchased a membership from your
business. - **Member Affiliate Rate** – The percentage of each sale paid to
affiliates who are already members of your business.


## Related topics

- [Affiliate Program](https://docs.whop.com/manage-your-business/growth-marketing/affiliate-program.md)
- [Set Up Custom Affiliates](https://docs.whop.com/affiliates/setup-custom.md)
- [Promote Your Business](https://docs.whop.com/affiliates/promote-your-business.md)
