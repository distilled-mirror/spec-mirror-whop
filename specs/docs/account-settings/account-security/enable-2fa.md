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

# Enable 2FA

> Add an extra layer of security to your Whop account by enabling two-factor authentication.

You can protect your Whop account by enabling two-factor authentication (2FA). After you turn it on, Whop logs you out of devices that refresh automatically to help keep your account secure.

## Enable two-factor authentication

1. Sign in to your account on Whop.
2. Go to [Account settings](https://whop.com/@me/settings/) or open [Security & Privacy](https://whop.com/@me/settings/security/) directly.
3. Select the **Security & Privacy** tab.
4. Under **Two-factor authentication**, choose **Authenticator App** (recommended) or **Text message**.

To add or change the phone number on your account, see [Update phone number](https://docs.whop.com/account-settings/personal-details/update-phone-number.md).

> [!TIP]
>
> Whop recommends using an authenticator app. It's more secure than SMS and
> helps reduce risks such as SIM swapping.


## Related topics

- [Team Management](https://docs.whop.com/manage-your-business/team-management/manage-team-roles.md)
- [SeQura Guide](https://docs.whop.com/payments-and-billing/financing/sequra-guide.md)
- [Adaptive Pricing](https://docs.whop.com/payments-and-billing/fees/adaptive-pricing.md)
