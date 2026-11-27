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

# Create Domain

> Adds a domain to your account with the capabilities you want.

Pass `registration` to buy the domain through Whop; it's the default when you pass no capability. Pay its `amount_due` at `purchase_url`, or pass `registration.payment_method_id` to charge a saved card. Whop then registers it, runs its DNS, and renews it every year while `auto_renew` is on.

Pass `registration.auth_code` to transfer in a domain you own at another registrar. The transfer starts once its `amount_due` is paid, or at once when `amount_due` is `null`. While `registration.phase` is `transferring`, its `issues` say what the transfer needs from you.

Pass `verification` to connect a domain you registered elsewhere: its `issues` list the TXT and routing records to publish. A subdomain of a domain this account bought through Whop takes `subdomain` by default, and Whop publishes its DNS records itself. Pass `website` with an `app_id` to serve that app on the domain.

To change a domain you already have, update it instead. Adding a domain this account deleted revives it under its original ID.



## OpenAPI

<!-- OpenAPI source: `post /domains`, docs.whop.com/openapi/api-v1-native.json (inlined by docs.whop.com; stripped on download; see specs/openapi.json) -->

## Related topics

- [Domains API](https://docs.whop.com/api-reference/domains/domain.md)
- [Enable Apple Pay and Google Pay](https://docs.whop.com/payments/apple-pay.md)
- [Domain created](https://docs.whop.com/api-reference/domains/domain-created.md)
