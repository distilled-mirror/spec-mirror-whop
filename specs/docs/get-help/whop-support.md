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

# Whop Support

> Get help any time from Whop AI, which can see your account and resolve most issues, or reach Whop support through the support page or by email.

**Whop AI** provides support at any time. It can see your account — memberships, payments, and payouts — and resolve most issues, from canceling a subscription to tracking down a missing Discord role. Whop AI routes complex conversations to the support team, which follows up by email when the conversation requires action.

> [card] [Chat with Whop Support](https://whop.com/support)
>
> Get help now — available at any time, every day.

You can also go to [whop.com/support](https://whop.com/support), or open **Help and support** from your profile menu.

You can also reach Whop support by email at **[support@whop.com](mailto:support@whop.com)**.

## More than support

Whop AI isn't just for fixing problems. If you have questions about Whop, want help building your business, or want to pull data or insights about your customers, ask it anything at [whop.com/chats/new](https://whop.com/chats/new).


## Related topics

- [Whop Partner Program Guide](https://docs.whop.com/partners.md)
- [Prohibited Businesses](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md)
- [Sales Tax and VAT](https://docs.whop.com/payments-and-billing/fees/taxes.md)
