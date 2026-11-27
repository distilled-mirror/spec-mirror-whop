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

# Build Whop Apps

> Ship an app that thousands of Whop businesses can install from the App Store.

A Whop app installs into a creator's community and runs inside it. Thousands of businesses already taking payments on Whop can install yours from the App Store, and Whop handles sign-in, billing, and mobile.

## What you can build

* **Courses and cohorts** members work through without leaving the community they bought into.
* **Live experiences** like events, chat-native games, and leaderboards that run against real membership.
* **Market data and trading tools** that stream to a paid audience.
* **Operational tools for the creator**, covering analytics, moderation, fulfillment, and onboarding.
* **Software you already sell**, listed as a Whop app to businesses already making money there.

## Core objects

App, OAuth grant, permission, app view. Each is defined in [Core Concepts](https://docs.whop.com/developer/concepts.md).

## Where to go next

> [card] [App views](https://docs.whop.com/developer/guides/app-views.md)
>
> Configure where your app appears.

> [card] [Authentication](https://docs.whop.com/developer/guides/authentication.md)
>
> Verify the current user and their access.

> [card] [OAuth](https://docs.whop.com/developer/guides/oauth.md)
>
> Add Sign in with Whop.

> [card] [Permissions](https://docs.whop.com/developer/guides/permissions.md)
>
> Request the access your app needs.

> [card] [Listing requirements](https://docs.whop.com/developer/apps/listing-requirements.md)
>
> What Whop checks before your app appears in the App Store.


## Related topics

- [Build Whop Apps with React Native](https://docs.whop.com/developer/guides/react-native.md)
- [App Store Listing Requirements](https://docs.whop.com/developer/apps/listing-requirements.md)
- [App Build](https://docs.whop.com/api-reference/legacy/app-builds/app-build.md)
