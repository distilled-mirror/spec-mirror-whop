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

# Platform Quickstart

> Embed a payout portal for your subaccounts with Whop Elements in minutes

# Embed a payout portal

### Congratulations!

You've embedded a payout portal in your application.

## Next steps

> [card] [Wallet elements](https://docs.whop.com/elements/latest/wallet/overview)
>
> Every wallet surface with its options, events, scopes, and styling.

> [card] [Hosted Payout Portal](https://docs.whop.com/developer/platforms/render-payout-portal.md#hosted-payout-portal)
>
> Skip embedding entirely and redirect users to a Whop-hosted payout portal.

> [card] [Manual Payouts](https://docs.whop.com/developer/platforms/manual-payouts.md)
>
> Programmatically trigger payouts, manage KYC verification, and control payout methods via the API.

> [card] [Enroll Subaccounts](https://docs.whop.com/developer/platforms/enroll-subaccounts.md)
>
> Learn how to create and manage Account objects for your subaccounts.

> [card] [Example Integration](https://docs.whop.com/developer/guides/example-integration.md)
>
> See checkout, subaccounts, and payouts working together in a full Next.js marketplace.


## Related topics

- [Marketplaces & Platforms](https://docs.whop.com/developer/marketplaces/overview.md)
- [Whop API Core Concepts](https://docs.whop.com/developer/concepts.md)
- [Platform Map](https://docs.whop.com/developer/platform-map.md)
