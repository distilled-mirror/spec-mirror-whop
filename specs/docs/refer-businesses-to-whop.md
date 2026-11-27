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

# Refer Businesses to Whop

> Earn 30% of Whop's profit from businesses you refer by sharing your referral link or inviting a specific business from the Whop partners page.

When you refer a business to Whop, you earn 30% of Whop's profit from that business. The [Whop Partner Program guide](https://docs.whop.com/partners.md) covers how to introduce a business, guide its setup, and get paid.

<video autoPlay src="https://mintcdn.com/whop/O2UveoaTiTL7ZPwK/how-to-videos/partners.mp4?fit=max&auto=format&n=O2UveoaTiTL7ZPwK&q=85&s=029fc5c68a5e0406f846383a93af1c25" alt="Refer businesses to Whop" />

Go to the [Whop partners page](https://whop.com/partners) to share your referral link, invite a specific business, and track your referrals.

## Share your referral link

Use the copy button on the partners page to get your referral link. The link follows this format:

```
https://whop.com/network/?a=[your_username]
```

The `a` parameter identifies your Whop username so Whop can attribute referred businesses to you.

## Refer a specific business

1. On the partners page, select **New referral**.
2. Enter the business name. You can also add its website and estimated annual revenue.
3. Enter the owner's email address, then select **Create & invite**.

Whop creates the business and emails the owner an invitation to take ownership. The business appears in your partners dashboard so you can track the referral.


## Related topics

- [Whop Partner Program Guide](https://docs.whop.com/partners.md)
- [Promote Your Business](https://docs.whop.com/affiliates/promote-your-business.md)
- [Affiliate Program](https://docs.whop.com/manage-your-business/growth-marketing/affiliate-program.md)
