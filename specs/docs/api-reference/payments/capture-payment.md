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

# Capture Payment

> Captures a card payment created with `capture: false` for its `total`. The payment must still be in `requires_capture` before `capture_expires_at`. An in-person payment's total can move away from its authorized amount when its line items change after the card was authorized. Above it, Whop captures the difference where Stripe allows it for the merchant's category, and otherwise raises the authorization first, which the card can decline. Capturing more or less than the authorized amount is off by default. Contact Whop support to turn it on for your account. Multiple captures are not supported.



## OpenAPI

<!-- OpenAPI source: `post /payments/{id}/capture`, docs.whop.com/openapi/api-v1-native.json (inlined by docs.whop.com; stripped on download; see specs/openapi.json) -->

## Related topics

- [Payments API](https://docs.whop.com/api-reference/payments/payment.md)
- [Replace a Payment Rule](https://docs.whop.com/api-reference/payment-rules/replace-a-payment-rule.md)
- [Retrieve a Payment Rule](https://docs.whop.com/api-reference/payment-rules/retrieve-a-payment-rule.md)
