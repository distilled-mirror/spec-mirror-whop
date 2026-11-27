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

# Create setup intent

> Save a buyer's payment method for later without charging it. Provide a confirmation token for a method the buyer just supplied, or an existing payment method to re-verify. The buyer may still have a step to complete — 3D Secure, a hosted enrollment, linking a bank account — so poll the setup intent's status endpoint for what to do next.

> [!NOTE]
>
> This page documents the Legacy API. For new integrations, use [Create Setup Intent](https://docs.whop.com/api-reference/setup-intents/create-setup-intent.md) in the Current API.

Required permissions:

* `payment:charge`
* `member:basic:read`
* `member:email:read`


## OpenAPI

<!-- OpenAPI source: `post /setup_intents`, docs.whop.com/openapi/api-v1-stable.json (inlined by docs.whop.com; stripped on download; see specs/openapi.json) -->

## Related topics

- [Create Setup Intent](https://docs.whop.com/api-reference/setup-intents/create-setup-intent.md)
- [Setup Intent](https://docs.whop.com/api-reference/legacy/setup-intents/setup-intent.md)
- [Setup Intents API](https://docs.whop.com/api-reference/setup-intents/setup-intent.md)
