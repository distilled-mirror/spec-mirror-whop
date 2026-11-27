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

# Pay Out Cashback

> Pays out cashback on demand from the authenticated platform's available USD balance to its direct subaccounts. Covers completed, unpaid card transactions created before the request, each under its latest matching rule, which must be funded by the authenticated platform. Filters combine, and an empty body includes every eligible transaction. Payouts process in the background and amounts are calculated then, so the response confirms queuing, not payment. If a payout fails for insufficient funds, add USD to the platform's balance. Requires `payout:transfer_funds`. Supports `Idempotency-Key`, and overlapping requests cannot pay the same card transaction twice.



## OpenAPI

<!-- OpenAPI source: `post /cashback_rules/payout`, docs.whop.com/openapi/api-v1-native.json (inlined by docs.whop.com; stripped on download; see specs/openapi.json) -->

## Related topics

- [Cashback Rules API](https://docs.whop.com/api-reference/cashback-rules/cashback-rule.md)
- [Create Cashback Rule](https://docs.whop.com/api-reference/cashback-rules/create-cashback-rule.md)
- [Connect a PayPal Payout Method](https://docs.whop.com/developer/guides/connect-paypal-payout-method.md)
