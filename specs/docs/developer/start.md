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

# What You Can Build with Whop

> Payments, payouts, identity, community, and distribution through one API. Hand the setup to your coding agent, or take your first payment in the sandbox.

Whop is one API for taking payments, paying people out, verifying identity, granting access, and embedding chat. You can build Whop into your own product, or ship an app that runs inside Whop. Everything below starts in the sandbox, where no real money moves.

## Build with your coding agent

Paste this into Claude Code, Cursor, Codex, or any coding agent. Replace the last line with what you're building.

```text Prompt theme={null}
Build my Whop integration. Build and test in the Whop sandbox. Production is for launch only.

Whop has two environments. Never mix their API keys, URLs, or webhook secrets:
- SANDBOX (build and test here, no real money): API https://sandbox-api.whop.com/api/v1, keys and webhooks at https://sandbox.whop.com/dashboard/developer, SDK environment Sandbox.
- PRODUCTION (launch only, real money): API https://api.whop.com/api/v1, keys and webhooks at https://whop.com/dashboard/developer, SDK environment Production, which is the SDK default.

1. Read https://docs.whop.com/skill.md and follow it.
2. Connect the Whop MCP server at https://mcp.whop.com/mcp to search the docs. It acts on PRODUCTION data, so don't use it to create or change anything.
3. Read WHOP_ENVIRONMENT (sandbox or production), WHOP_API_KEY, and WHOP_WEBHOOK_SECRET from the server's environment. Choose the SDK environment from WHOP_ENVIRONMENT, and default to sandbox when it's unset.
4. Follow https://docs.whop.com/developer/quickstart.md to make the first call, create a checkout, and handle the payment.succeeded webhook.
5. Ask me before running anything against production.

What I'm building:
```

Sandbox and production are separate Whop environments with separate accounts, keys, and webhooks, and their keys look identical. The prompt names both so your agent builds against the sandbox and knows exactly what changes at launch. To give your agent Whop's conventions permanently, install the skill with `npx skills add https://docs.whop.com`. [AI Tools](https://docs.whop.com/developer/ai/overview.md) covers the MCP server, plugins, and per-client setup.

## Or start by hand

> [card] [Quickstart](https://docs.whop.com/developer/quickstart.md)
>
> Get a sandbox key, make your first call, pay a checkout with a test card, and receive the webhook. About 10 minutes.

## Choose your path

> [card] [You sell a subscription or a one-time product](https://docs.whop.com/developer/guides/accept-payments.md)
>
> Checkout links or the Checkout element. First payment in under an hour.

> [card] [You owe money to people who aren't you](https://docs.whop.com/developer/platforms/manual-payouts.md)
>
> Send payouts, embed the payout portal, top up balances.

> [card] [You take a cut of what other people sell](https://docs.whop.com/developer/platforms/quickstart.md)
>
> Enroll subaccounts, collect on their behalf, pay them out.

> [card] [You want distribution, not just infrastructure](https://docs.whop.com/developer/guides/app-views.md)
>
> Ship an app that runs inside Whop, where businesses install it. Start with the CLI, not an API key.

> [card] [Your product needs people talking to each other](https://docs.whop.com/developer/guides/chat/quickstart.md)
>
> Chat, DMs, and forums, embedded in your product.

> [card] [You want to see everything](https://docs.whop.com/developer/platform-map.md)
>
> Every capability, and whether it comes as an API, an element, or a hosted page.


## Related topics

- [Build Whop Apps](https://docs.whop.com/developer/apps/overview.md)
- [Whop Partners API](https://docs.whop.com/developer/partners/overview.md)
- [Whop API Quickstart](https://docs.whop.com/developer/quickstart.md)
