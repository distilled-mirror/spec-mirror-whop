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

# Report a Bad Actor

> Report other users or businesses for harassment, hateful content, scams, or other Terms of Service violations.

You can report other users or creators/whops who are harassing others, promoting hateful content, scamming, or otherwise violating Whop's Terms of Service.

## How to report a business or whop

Report creators or whops for inappropriate content or behavior by:

1. Open their **Overview** page (store page) and scroll to the bottom.
2. Select **Report this company**. Whop will notify the appropriate team.

You can also report creators or whops at any time through [Whop live chat support](https://whop.com/support).

## How to report a user

Report other users for harassment, hateful content, scams, or other behavior that breaks Whop's Terms of Service by contacting [Whop live chat support](https://whop.com/support).

To stop that user from contacting you, you can [block them](#how-to-block-a-user) from your account or in the conversation (e.g. in chat or DMs).

## How to block a user

You can block other users inside any whop you're part of.

1. Sign in to your account at [whop.com](https://whop.com).

2. Find the user you want to block in one of these ways:
   * **From any app:** Select the people icon on the right-hand side to open the user list (or use the sidebar if it's already visible). Scroll to find the user or search for their username.
   * **From Chat:** If the user has sent a message in the Chat app, you can find them there.
   * **From Overview:** In **Your whops**, open the whop, then go to **Overview**. Select the membership count to open the member list. Scroll to find the user.

3. Select the user's **profile photo**. A profile window will open.

4. Select the **three dots** icon (⋮).

5. Select **Block**.


## Related topics

- [Prohibited Businesses](https://docs.whop.com/trust-and-safety/trust-safety-overview/what-is-not-allowed-on-whop.md)
- [Community Guidelines](https://docs.whop.com/trust-and-safety/trust-safety-overview/community-guidelines.md)
- [Contact a Merchant](https://docs.whop.com/memberships-and-access/cancellations-and-refunds/contact-a-merchant.md)
