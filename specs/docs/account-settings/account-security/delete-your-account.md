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

# Delete Your Account

> Permanently delete your Whop user account after you delete your businesses and withdraw your balance.

This process deletes your **entire Whop user account**. It doesn't delete just one business or whop. If you only want to remove one of your businesses, see [Manage products](https://docs.whop.com/manage-your-business/products/manage-products.md) for options.

## How to delete your entire Whop account

1. Sign in to your account on Whop.
2. Go to [Account settings](https://whop.com/@me/settings/).
3. Select the **Danger zone** tab.
4. Select **Delete account**. A form will appear.
5. Type **delete** into the text box and select **Delete account**.

> [!WARNING]
>
> Cancel all memberships before you delete your Whop account. See
> [Cancel a
> subscription](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/cancel-a-subscription.md)
> to learn how to cancel a membership.


## Related topics

- [Cancel a Subscription](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/cancel-a-subscription.md)
- [Access a Discord Server](https://docs.whop.com/memberships-and-access/access-discord-server/access-a-discord-server.md)
- [Payment Health](https://docs.whop.com/trust-and-safety/account-health/payment-health.md)
